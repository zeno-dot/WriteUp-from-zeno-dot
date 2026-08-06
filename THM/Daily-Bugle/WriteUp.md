---
title: "Daily Bugle — TryHackMe"
---

# Daily Bugle — TryHackMe

<img width="1631" height="206" alt="Screenshot from 2026-08-06 08-56-23" src="https://github.com/user-attachments/assets/b6bfc06a-5d60-4b39-bbed-c88c80d44d06" />

<div class="wu-meta">
<span>Сложность <b>Сложная</b></span>
<span>Платформа <b>TryHackMe</b></span>
</div>

<div class="wu-flags">
<a href="#flag1">🚩 Флаг 1</a>
<a href="#flag2">🚩 Флаг 2</a>
</div>

Доброго времени суток, впервые на обзоре прохождение сложной комнаты на THM. Были использованы инструменты: nmap, gobuster, joomscan, nuclei, sqlmap, hashcat. Работа с реверс-шеллом из CMS Joomla через подмену темы. Дважды повышали привилегии на машине: путём поиска внутренней информации и использования мисконфигурации в yum. Начнём же разбор.

## Разведка

Получив Attacker machine и Lab machine, мы начинаем с самой базовой команды для проверки, с чем мы вообще работаем.

<img width="884" height="460" alt="Screenshot from 2026-08-06 09-30-06" src="https://github.com/user-attachments/assets/2cc83d44-2470-4532-bb85-7ba27e5275a5" />

Стандартный вектор через веб. Первый вопрос у нас звучит так: "Access the web server, who robbed the bank?". Зайдём на 80 порт и посмотрим содержимое.

<img width="1925" height="1170" alt="Screenshot from 2026-08-06 09-20-54" src="https://github.com/user-attachments/assets/71ad2ee4-c8b0-4a4c-ae2e-e424d57d316f" />

И первый ответ на вопрос сразу понятен. Но тут не всё так прямо и очевидно: вариант Spider-Man не проходит, зато spiderman проходит. Уже не в первый раз на THM встречаю такое странное поведение, но тут хотя бы в рамках логичного. Изучение веба ничего не дало, по сути там только форма логина и пост об ограблении. Используем gobuster.

<img width="1518" height="1227" alt="Screenshot from 2026-08-06 10-20-51" src="https://github.com/user-attachments/assets/0bca8dee-4ff1-4340-9147-f5451071ee7c" />

Ничего дельного на самом деле нет. После проверки всех путей из полезной информации только то, что используется CMS Joomla. А значит, запустим joomscan.

<img width="1518" height="1334" alt="Screenshot from 2026-08-06 10-22-45" src="https://github.com/user-attachments/assets/3b4475d1-a7e9-4412-a86d-4809f8cf20e8" />

И мы можем ответить на следующий вопрос "What is the Joomla version?", и это версия 3.7.0. Т.к. нет никаких дельных векторов для изучения (на странице ничего нет, подсказок нет, все пути без полезной информации), смотрим наличие CVE.

<img width="911" height="273" alt="Screenshot from 2026-08-06 10-26-35" src="https://github.com/user-attachments/assets/ebafb21b-ddf8-4e8f-a4b4-edf332cb0160" />
<img width="1422" height="513" alt="Screenshot from 2026-08-06 10-26-47" src="https://github.com/user-attachments/assets/0f11e2b9-b33a-43e1-be76-fd76bc250971" />

И мы с ходу видим критическую уязвимость с оценкой 9.8. Чтобы верифицировать эту CVE, в нашем случае поможет nuclei.

<img width="1647" height="453" alt="Screenshot from 2026-08-06 10-28-55" src="https://github.com/user-attachments/assets/57090c22-b0f7-4658-97d8-ad38e589026d" />

И мы подтвердили наличие уязвимости. Значит, у нас SQL injection. Судя по запросу от nuclei, мы имеем дело с типом error-based, то есть пайлод, который намеренно вызывает ошибку, раскрывающую внутреннюю информацию из СУБД. Переходим в sqlmap и пробуем вытащить пользователей.

## SQLi и дамп базы

<img width="1666" height="707" alt="Screenshot from 2026-08-06 10-45-57" src="https://github.com/user-attachments/assets/30c47274-161f-4735-9db7-ddadecb0f68c" />

Указываем уязвимый параметр, даём полную глубину проверки, пропускаем проверки на WAF, ставим -v 3, чтобы видеть реакцию на пайлоды (500 ошибка — это очень хороший знак на возможную уязвимость), игнорируем Set-Cookie от сервера (помогает обходить защиты, построенные на куках), СУБД мы знаем ещё со скана nmap, рандом-агент также помогает обходить некоторые фильтры, технику выставили на error, т.к. это рабочий вариант под текущую CVE (Union точно нет из-за конструкции ORDER BY при формировании запроса на параметре list[fullordering]). По сути команду можно урезать полностью и оставить чисто sqlmap -u "...", но будем считать, что решил выпендриться ;)

<img width="1666" height="707" alt="Screenshot from 2026-08-06 10-46-29" src="https://github.com/user-attachments/assets/69053387-13af-4a0d-8305-ede1e3fec712" />

Возможный вектор с error найден.

<img width="1666" height="919" alt="Screenshot from 2026-08-06 10-46-50" src="https://github.com/user-attachments/assets/c1a01f87-9687-4d4d-a0bb-4b163dafd887" />

Проверка на фолс пройдена, и мы получаем error-based. Достаём названия баз данных.

<img width="1666" height="981" alt="Screenshot from 2026-08-06 10-47-39" src="https://github.com/user-attachments/assets/4e5e3b3a-b091-47cb-81fb-a9b7603b01c3" />

Нас явно интересует joomla. Достаём таблицы базы данных joomla.

<img width="1663" height="850" alt="Screenshot from 2026-08-06 12-01-25" src="https://github.com/user-attachments/assets/4be5c653-1281-439d-a9fb-18cd8c03fdc7" />
<img width="1663" height="1059" alt="Screenshot from 2026-08-06 12-01-43" src="https://github.com/user-attachments/assets/f7480d56-5f38-451f-973b-b2ffc92b3c0b" />

Нас интересует "#__users" — дампим.

<img width="1688" height="796" alt="Screenshot from 2026-08-06 12-02-36" src="https://github.com/user-attachments/assets/d56ebae5-41fb-4156-a71a-815cf2d1b174" />
<img width="1688" height="1052" alt="Screenshot from 2026-08-06 12-02-51" src="https://github.com/user-attachments/assets/7c1a0960-2e62-4ff5-88f9-159e5e52cd99" />

Пришлось попутно пробрутить названия столбцов. В итоге у нас есть логин, почта и хэш. Пробуем дехэшить и доставать пароль.

<img width="1688" height="186" alt="Screenshot from 2026-08-06 12-07-03" src="https://github.com/user-attachments/assets/c1df65d7-1bc1-4da0-a62a-0e228ee5099b" />
<img width="1688" height="1071" alt="Screenshot from 2026-08-06 12-07-18" src="https://github.com/user-attachments/assets/b69941c8-65b2-4711-ad56-1f91e115c5f4" />

Мы получаем пароль, и это также ответ на вопрос площадки "What is Jonah's cracked password?", а также есть логин. Входим в админку joomla на 80 порту.

## Админка и реверс-шелл

<img width="2563" height="988" alt="Screenshot from 2026-08-06 12-09-29" src="https://github.com/user-attachments/assets/be78029d-fc0b-4a91-ad65-dfff79e75712" />

Начинаем изучать всё, что есть внутри админки, и искать вектор для продвижения.

<img width="2562" height="741" alt="Screenshot from 2026-08-06 12-41-21" src="https://github.com/user-attachments/assets/9e014c77-c9c3-4027-bc08-3664c25d34a6" />

<img width="2562" height="972" alt="Screenshot from 2026-08-06 12-41-32" src="https://github.com/user-attachments/assets/bd7a48ab-ae94-4e42-b696-0ed439ef095a" />

<img width="2562" height="972" alt="Screenshot from 2026-08-06 12-42-04" src="https://github.com/user-attachments/assets/2f70ce76-8c0b-461c-b7fa-068fbfe5a432" />

<img width="2551" height="1171" alt="Screenshot from 2026-08-06 12-58-23" src="https://github.com/user-attachments/assets/9ef4ea5c-8863-4a58-9c13-bc7bc9c260b4" />

Что мы делаем? В ходе 5-минутных поисков в итоге находим самый дефолтный вектор — изменение/создание файла внутри template. Мы создаём файл с реверс-шеллом и пробуем делать проброс.

<img width="692" height="221" alt="Screenshot from 2026-08-06 12-59-09" src="https://github.com/user-attachments/assets/7130099b-e3f0-4e3c-b1c6-22db2d3de691" />
<img width="1906" height="963" alt="Screenshot from 2026-08-06 12-59-33" src="https://github.com/user-attachments/assets/b7020ed3-bf01-4e87-a323-f3921a8158b6" />
<img width="630" height="259" alt="Screenshot from 2026-08-06 12-59-41" src="https://github.com/user-attachments/assets/8a5d6962-d27c-4dcd-b084-22a7960c23d9" />

Мы внутри. Делаем TTY и изучаем машину.

<img width="630" height="259" alt="Screenshot from 2026-08-06 13-00-58" src="https://github.com/user-attachments/assets/c22810c1-d701-433e-8ed6-5ef0ef58e695" />

<img width="1258" height="597" alt="Screenshot from 2026-08-06 13-01-51" src="https://github.com/user-attachments/assets/0a82e6cd-5df3-4b1f-9d37-62f98df96dd7" />

<img width="1258" height="597" alt="Screenshot from 2026-08-06 13-09-00" src="https://github.com/user-attachments/assets/68787fd1-f0f1-44f3-8e6e-8a5bad1eccb1" />

Пароль от учётки админки не подходит. Но в passwd мы видим Jonah, и его пользователь в машине jjameson, ищем ещё варианты паролей.

<img width="1258" height="597" alt="Screenshot from 2026-08-06 13-14-51" src="https://github.com/user-attachments/assets/513101e5-5a47-457f-ba0a-f8061713bd18" />

Пробуем как пароль для пользователя jjameson.

<img width="1258" height="597" alt="Screenshot from 2026-08-06 13-15-30" src="https://github.com/user-attachments/assets/c24c3d9a-6a1d-4f10-bfbb-b5703e5e084a" />

Успех. Ищем флаг.

<a id="flag1" class="anchor"></a>
<img width="1258" height="597" alt="Screenshot from 2026-08-06 13-18-44" src="https://github.com/user-attachments/assets/10448492-836a-43e0-91cd-040460ebaac8" />

## Повышение привилегий до root

Дальше ищем вектор для рута.

<img width="1258" height="597" alt="Screenshot from 2026-08-06 13-19-05" src="https://github.com/user-attachments/assets/0317b572-66ac-490e-bc4f-f9bdd78f85d6" />

Видим строку, которая дозволяет yum выполнять команды от имени рута. Это стандартный вектор, описанный в gtfobins (https://gtfobins.org/gtfobins/yum/). Если по-простому, то мы используем мисконфиг, который при загрузке вредоносного rpm даёт возможность выполнить любую команду от имени рута, в нашем случае это получение оболочки рута.

<img width="1274" height="1162" alt="Screenshot from 2026-08-06 13-40-45" src="https://github.com/user-attachments/assets/6a7a9685-ec0b-4db4-8d46-f1aaf21000e3" />

Отмечу, что данный набор команд нужен был именно мне, т.к. я проводил попутно тесты. Поэтому добавил переменные, которые визуально упрощают чтение и понимание, и открыл свой порт 8000, т.к. по умолчанию у меня всё закрыто. В общем, открываем gtfobins и изучаем более детально, как проходит атака, или читаем в самом конце разбора, а мы идём далее.

<img width="1274" height="1162" alt="Screenshot from 2026-08-06 13-41-33" src="https://github.com/user-attachments/assets/af240afa-ec76-483e-abcf-8742529d6777" />

С атакующего сервера мы скачиваем на уязвимую машину нужный нам файл, ставим его через sudo yum localinstall, и при установке его скрипт выполняется от root, поднимая нам оболочку рута. И забираем последний флаг.

<a id="flag2" class="anchor"></a>
<img width="1274" height="348" alt="Screenshot from 2026-08-06 13-42-09" src="https://github.com/user-attachments/assets/44273089-fbbc-45df-a2f9-0765ccdda963" />

## Мнение о комнате

Комната вышла интересной. В меру "сложной", но на деле тут просто много мелких действий. Из сложного может быть только концовка, поэтому даю небольшое объяснение, что вообще произошло. sudo разрешает jjameson только один бинарник — yum, он сам по себе не уязвимый, это критический мисконфиг (ошибка настройки или лишних прав). yum — это консоль управления пакетами, сам по себе он шелл дать не может. Но yum может устанавливать RPM-пакеты, а в этих пакетах можно оставить скрипт, который выполнится rpm от root (т.к. мы можем вызвать yum от root). Скрипт в свою очередь нам и даёт баш рута. Это может быть сложным в понимании, но на деле всё просто: сделали уязвимый файл на атакующей машине и прокинули его на машину жертвы, вся суть. В общем, комната хорошая, заставляет потыкать разные инструменты, почитать за суть уязвимостей и ошибок в настройках.
