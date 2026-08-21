---
title: "Wonderland — TryHackMe"
---

# Wonderland — TryHackMe

<img width="1746" height="286" alt="Screenshot from 2026-08-21 07-26-10" src="https://github.com/user-attachments/assets/993e7214-de0c-447c-84cf-f5d8f45c52f5" />

<div class="wu-meta">
<span>Сложность <b>Средняя</b></span>
<span>Платформа <b>TryHackMe</b></span>
</div>

<div class="wu-flags">
<a href="#flag1">🚩 Флаг 1</a>
<a href="#flag2">🚩 Флаг 2</a>
</div>

Доброго времени суток, на прохождении комната средней сложности "Страна чудес". Комната вышла неоднозначной и в некоторых местах непонятной, также, скорее всего, имелась проблема с флагами. Обо всём этом читаем далее.

## Разведка

Получив Lab machine, мы начинаем с самой базовой команды для проверки, с чем мы вообще работаем.

<img width="1117" height="359" alt="Screenshot from 2026-08-20 12-33-03" src="https://github.com/user-attachments/assets/0a6a75c9-29a1-4d44-bb43-461dc7d03a51" />

Стандартное развитие через веб, смотрим и изучаем.

<img width="1343" height="1053" alt="Screenshot from 2026-08-20 12-36-43" src="https://github.com/user-attachments/assets/2acf4621-fe6d-4d00-aecb-0321b01ba915" />

## Веб и первый доступ

Есть статичная страница без каких-то ссылок. Решаю немного изучить код.

<img width="1343" height="1053" alt="Screenshot from 2026-08-20 12-36-54" src="https://github.com/user-attachments/assets/17ffca4b-642a-4bd0-abba-144562f7c4b7" />

Можно понять, что страница подгружает картинку из директории img. Проверяем, что там и открыта ли она нам.

<img width="1691" height="1360" alt="Screenshot from 2026-08-20 12-37-08" src="https://github.com/user-attachments/assets/783f1b38-f457-4b1a-a978-c44ce7c35041" />

<img width="1691" height="1360" alt="Screenshot from 2026-08-20 12-37-18" src="https://github.com/user-attachments/assets/58a0620a-ff4f-424d-9b5b-c4be943337b1" />

<img width="1691" height="1360" alt="Screenshot from 2026-08-20 12-37-25" src="https://github.com/user-attachments/assets/d403afd1-28ad-4cba-872f-48f8501a7bb8" />

Внутри находятся вот такие 3 картинки. Исходя из этого, делаю вывод, что есть скрытые от глаза директории. Смотрим в gobuster.

<img width="1260" height="626" alt="Screenshot from 2026-08-20 12-40-06" src="https://github.com/user-attachments/assets/5b5da40c-f46d-4d8d-a5c2-2b4e64af32c2" />

И тут весьма неоднозначная картина: из всех директорий нам подходит для изучения только /r.

<img width="1475" height="964" alt="Screenshot from 2026-08-20 12-40-15" src="https://github.com/user-attachments/assets/0801528d-e930-4a7c-85de-d5fb09baf64d" />

При этом в самой директории ничего особенного не написано. Решил полностью перечитать весь текст и найти подсказку. И, судя по тексту, нужно найти кролика? Проверка на директорию /rabbit ничего не дала, в ходе перебора вариантов был найден такой вектор.

<img width="1475" height="964" alt="Screenshot from 2026-08-20 12-40-21" src="https://github.com/user-attachments/assets/ad422337-7664-4433-8fe1-c150e3667fb0" />

<img width="1475" height="964" alt="Screenshot from 2026-08-20 12-40-28" src="https://github.com/user-attachments/assets/7186b282-585e-482e-8f04-90189adc2a40" />

<img width="1475" height="964" alt="Screenshot from 2026-08-20 12-40-34" src="https://github.com/user-attachments/assets/423fd7cc-85f5-4f48-8486-90f5bb46e060" />

<img width="1475" height="964" alt="Screenshot from 2026-08-20 12-40-40" src="https://github.com/user-attachments/assets/cd2ef00e-7dab-44d6-a7e9-bfb4e753812d" />

<img width="1475" height="1076" alt="Screenshot from 2026-08-20 12-40-49" src="https://github.com/user-attachments/assets/5af6a7f6-fb1b-4b9d-b6be-8c3e94f02dfc" />

И мы вышли на очередной текст-ребус. Были многочисленные попытки дальнейшего брута директорий в разных вариациях и местах. В итоге спустя некоторое время решил опять изучить код страницы.

<img width="1475" height="1076" alt="Screenshot from 2026-08-20 13-24-29" src="https://github.com/user-attachments/assets/773f9ffe-85c3-4e22-938b-db8106aed406" />

Совсем непримечательная скрытая строка, видимо, с кредами, вопрос от чего? Нет ни админки, да и в целом ничего. Пробую вариант с подключением по ssh.

<img width="1100" height="484" alt="Screenshot from 2026-08-20 13-25-06" src="https://github.com/user-attachments/assets/ade621fc-af42-4497-95ed-fc4b536c4170" />

И это оказывается верным решением. Ищу первый флаг.

<a id="flag1" class="anchor"></a>
<img width="1272" height="1150" alt="Screenshot from 2026-08-20 13-26-21" src="https://github.com/user-attachments/assets/9ad4765d-a481-453e-b888-812ed21f0c81" />

И тут сразу вопрос: что тут делает флаг от рута? Забрать мы его, естественно, не можем, т.к. по правам на чтение и запись только для рута и доступен, а соседний файл пайтон не менее странный. Решаю искать вектор до рута, т.к. флаг это условный чекпоинт.

## Повышение привилегий

<img width="1132" height="270" alt="Screenshot from 2026-08-20 13-30-53" src="https://github.com/user-attachments/assets/8b19440f-50bd-43e0-bd1d-252bc373f64c" />

Тут мы выяснили, что этот пайтон-файл можно запустить с правами sudo от пользователя rabbit. Начал разбор пайтон-файла, т.к. его редактировать способен исключительно пользователь рут, коим мы не являемся.

<img width="869" height="174" alt="Screenshot from 2026-08-20 13-36-33" src="https://github.com/user-attachments/assets/f44f0505-d023-4eb3-93d8-ba1c1290ff66" />

Решение вышло в целом простым. Подмена "библиотеки": т.к. пайтон-файл ссылается на random.py, мы создаём рядом этот файл и прокидываем команду на получение оболочки от пользователя rabbit. Изучаем дальше.

<img width="1488" height="997" alt="Screenshot from 2026-08-20 13-38-06" src="https://github.com/user-attachments/assets/e62690d4-6775-4fed-98be-d372f6f3d0f1" />

Теперь мы получили на первый взгляд битый файл. При более точном изучении выяснилось, что это ELF-бинарник с setuid от пользователя hatter. По сути, используя этот бинарник, мы можем попасть на чаепитие к hatter прямо в его учётку.

<img width="819" height="220" alt="Screenshot from 2026-08-20 13-41-58" src="https://github.com/user-attachments/assets/df782f6e-f00a-4a33-b523-67b0b5b19345" />

Осматриваем окружение.

<img width="823" height="248" alt="Screenshot from 2026-08-20 13-42-28" src="https://github.com/user-attachments/assets/f5059225-2c22-425d-b787-53f6d9df8920" />

Получаем пароль пользователя, пробуем получить sudo.

<img width="919" height="555" alt="Screenshot from 2026-08-20 13-45-40" src="https://github.com/user-attachments/assets/1da89064-9e41-4cc3-b33d-f84b1fd9141f" />

По sudo -l нас отшили, в суид-бинарях ничего, но вот getcap нам даёт вектор через perl. Что это такое? cap_setuid+ep даёт нам возможность через perl выдать нам UID любого пользователя, и в том числе рута. По сути своей CAP_SETUID даёт выполнить системный вызов setuid(), но эта привилегия должна быть исключительно у рута, в данном случае можно это считать мисконфигом. В общем, меняем свой UID на рута.

<img width="919" height="97" alt="Screenshot from 2026-08-20 13-46-48" src="https://github.com/user-attachments/assets/8477917c-f1cd-4fca-bb6c-88a7ab23d5f3" />

И успешно получили UID root. Зайдём глянуть, что там у рута в директории.

<a id="flag2" class="anchor"></a>
<img width="708" height="270" alt="Screenshot from 2026-08-20 13-47-51" src="https://github.com/user-attachments/assets/d7053df3-5d5f-4cb5-a665-dde97f1ba01f" />

И тут почему-то не файл рут, а юзер. В любом случае оба файла успешно забраны, и конечная цель в получении рута была выполнена.

## Мнение о комнате

По сути это полная вакханалия. Поиск директорий через многочисленные / и поиск ssh в коде страницы это уже странно, но внутри машины всё ещё более странно. Не говоря о том, что файлы перепутаны, т.к. в рут-директории файл юзер могут читать все, в том числе первый полученный пользователь alice. Даже способ продвижения сделали нетипичным. Подмена вызова в пайтоне, странный бинарник и не менее странная дыра для рута. В целом комната и вышла странная. Получить рута оказалось проще, чем найти креды и нужную директорию в вебе. Такие комнаты уже сложно отнести к формату "средняя", это уже ближе к сложному, т.к. до многого нужно допереть. Это сильно отличается от многих комнат, где всё из разряда пришёл - увидел - победил. Несмотря на всю странность, комната сделана действительно хорошо.
