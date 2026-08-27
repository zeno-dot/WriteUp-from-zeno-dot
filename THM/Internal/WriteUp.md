---
title: "Internal — TryHackMe"
---

# Internal — TryHackMe

<img width="1177" height="232" alt="Screenshot from 2026-08-27 09-41-24" src="https://github.com/user-attachments/assets/9b3c1146-bdff-43fe-8088-3cc2b7edb1f1" />

<div class="wu-meta">
<span>Сложность <b>Сложная</b></span>
<span>Платформа <b>TryHackMe</b></span>
</div>

<div class="wu-flags">
<a href="#flag1">🚩 Флаг 1</a>
<a href="#flag2">🚩 Флаг 2</a>
</div>

Доброго времени суток, на прохождении сложная комната Internal. Стандартная хорошая комната для отработки навыков поиска и понимания работы сервисов. Сразу скажу, что 1 раз машина упала, поэтому в моменте у неё будет другой айпи. Начинаем

## Разведка

Получив Lab machine, мы начинаем с самой базовой команды для проверки, с чем мы вообще работаем.

<img width="986" height="401" alt="Screenshot from 2026-08-27 07-22-42" src="https://github.com/user-attachments/assets/012824db-27f4-4878-8cda-ac14f67c0f80" />

Стандартное развитие через веб, смотрим и изучаем.

<img width="1136" height="1333" alt="Screenshot from 2026-08-27 07-25-17" src="https://github.com/user-attachments/assets/df7e7196-1686-468c-8789-729081ec5cc6" />

Ничего толкового не видим, смотрим по gobuster

<img width="1320" height="726" alt="Screenshot from 2026-08-27 07-25-24" src="https://github.com/user-attachments/assets/1628d25a-c36b-4393-98b8-3ec6bb6ee882" />

Исследуем найденные точки, но сразу видно, что самый вероятный вектор будет через WP

<img width="1442" height="1253" alt="Screenshot from 2026-08-27 07-25-33" src="https://github.com/user-attachments/assets/6d6371f2-4670-4e1d-b067-0d62aa93b802" />

<img width="1209" height="726" alt="Screenshot from 2026-08-27 07-25-52" src="https://github.com/user-attachments/assets/5717631b-0334-4b71-bbd1-b80296d51fae" />

<img width="1464" height="968" alt="Screenshot from 2026-08-27 07-26-03" src="https://github.com/user-attachments/assets/8a3790e8-ca92-49df-a9ea-efb616f6522d" />

Всё выглядит крайне плачевно. Изучаю, в чём причина такого "особенного" веба

<img width="1123" height="608" alt="Screenshot from 2026-08-27 07-27-09" src="https://github.com/user-attachments/assets/bdc22d2f-c11a-412f-9dce-26f21dfefece" />

В предисловии было указано о внесении корректив в файл hosts и указан рабочий домен. Делаем всё корректно

<img width="886" height="76" alt="Screenshot from 2026-08-27 07-30-19" src="https://github.com/user-attachments/assets/2b3e5b91-1081-4e40-a84f-9e8719771bb5" />

Проверяем тачку

<img width="1444" height="1358" alt="Screenshot from 2026-08-27 07-30-32" src="https://github.com/user-attachments/assets/0d13c3e6-7e59-441b-ad2e-15683149792a" />

Дизайн вернулся, продолжаем работу дальше

<img width="1444" height="1358" alt="Screenshot from 2026-08-27 07-30-54" src="https://github.com/user-attachments/assets/3c30f1ae-3d37-435e-95d4-16836fe32c2b" />

WP доступно для логина, начнём с базовой проверки известных пользователей

<img width="1420" height="566" alt="Screenshot from 2026-08-27 07-44-00" src="https://github.com/user-attachments/assets/c6710ce5-270f-4225-baab-ca2efb74b666" />

Есть только один пользователь - admin. Пробуем через hydra пробрутить пароль, ранее мы уже разбирали всю методику более подробно, поэтому без лишних объяснений пробуем подобрать пароль.

<img width="1748" height="370" alt="Screenshot from 2026-08-27 08-10-45" src="https://github.com/user-attachments/assets/ca881ac5-1d66-4987-85c6-f3684758970a" />

Пароль успешно получен, заходим

<img width="1198" height="765" alt="Screenshot from 2026-08-27 08-11-11" src="https://github.com/user-attachments/assets/2e64939f-466c-49db-816e-3e9908890ea1" />

Когда сходу не пустило внутрь, я уже успел подумать, что придётся как-то учётку почты получать, но нет. Просто жмём "Remind me later" и идём далее

<img width="1767" height="1196" alt="Screenshot from 2026-08-27 08-14-07" src="https://github.com/user-attachments/assets/3925dbb8-354b-4a87-ba7a-b2e4075f8a53" />

И мы успешно зашли. Теперь самая стандартная практика с подменой содержимого php файла и проброса ревёрс-шелла

<img width="485" height="159" alt="Screenshot from 2026-08-27 08-15-03" src="https://github.com/user-attachments/assets/41122ab7-c803-48ad-9a48-abf88132616c" />

<img width="1578" height="1153" alt="Screenshot from 2026-08-27 08-22-01" src="https://github.com/user-attachments/assets/d1ef6457-7986-4036-87ea-31cd2f3fb63e" />

Не забываем сохранить, дёргаем url и спокойно заходим. Данный метод проброса является обычным и уже рассматривался ранее. Поэтому если что-то непонятное, открывайте предыдущий разбор на этом месте - https://zeno-dot.github.io/WriteUp-from-zeno-dot/THM/Mr-Robot-CTF/WriteUp.html#%D0%B4%D0%BE%D1%81%D1%82%D1%83%D0%BF-%D0%BA-%D0%B0%D0%B4%D0%BC%D0%B8%D0%BD%D0%BA%D0%B5

<img width="1088" height="211" alt="Screenshot from 2026-08-27 08-22-12" src="https://github.com/user-attachments/assets/6e198aa8-702a-49f0-8479-cd8ab8fc971a" />

Вход успешен, поднимаем TTY для удобной работы и ищем флаг

<img width="1211" height="749" alt="Screenshot from 2026-08-27 08-25-13" src="https://github.com/user-attachments/assets/83a52943-5e9b-48b9-afd8-34e86bea5f39" />

До флага путь пока что закрыт, ищем способ попасть в пользователя aubreanna. Т.к скриншот точки утерян, а проходить заново не особо хочется, то расписываю способ получения, который максимально лёгкий. cd /opt после пишем ls -la и видим файл и папку (с названием контейнера, но зайти не можем из-за малых прав). В самом файле txt лежат креды, по которым мы без проблем заходим

<img width="1063" height="491" alt="Screenshot from 2026-08-27 08-35-44" src="https://github.com/user-attachments/assets/4fe1725d-6184-4123-a71c-1f5feb5bb9f6" />

Успешный вход выполнен, забираем флаг

<a id="flag1" class="anchor"></a>
<img width="1063" height="491" alt="Screenshot from 2026-08-27 08-36-04" src="https://github.com/user-attachments/assets/35a6e474-e8b5-430d-b04e-9c812f2dc543" />

Теперь смотрим соседний файл

<img width="1063" height="491" alt="Screenshot from 2026-08-27 08-41-56" src="https://github.com/user-attachments/assets/fd8142f7-d5ea-4d37-96f5-c8a77b983a6e" />

Локально на машине поднят jenkins на порту 8080, чтобы было удобнее, пробросим прокси-соединение для того, чтобы мы могли спокойно зайти на веб

<img width="1102" height="517" alt="Screenshot from 2026-08-27 08-43-56" src="https://github.com/user-attachments/assets/3f3106c1-d494-4674-979c-a7e109a85bfa" />

Открыли мы на порту 8888, туда и зайдём

<img width="1217" height="944" alt="Screenshot from 2026-08-27 08-44-02" src="https://github.com/user-attachments/assets/242437ce-cad8-4c44-9023-1470f56627ad" />

Обратите внимание, что это локалхост. Ну а способ получения стандартный брутфорс. В этот раз я решил идти не через самый лёгкий путь. По сути можно было просто запустить гидру по локалхост 8888, но что если такой возможности нет и при этом нет возможности установить ту же гидру?

<img width="1519" height="69" alt="Screenshot from 2026-08-27 09-13-48" src="https://github.com/user-attachments/assets/c04f4801-c7a2-4347-b06d-5601799f5bd3" />

Кратко объясняю. По сути использовали стандартный инструмент в цикле. Юзера взяли стандартного по умолчанию - admin. Файл прокинул заранее, типичный рок ю. Указан эндпоинт проверки учётки, и при этом запуск из самой машины. В общем, бюджетный вариант гидры. Подобное, к слову, спокойно составит любая нейронка. В целом пароль подобран, заходим

<img width="1755" height="1094" alt="Screenshot from 2026-08-27 09-20-37" src="https://github.com/user-attachments/assets/c22663d8-6b73-4746-92d7-f90afdc85069" />

Дальше вектор опять известный. В jenkins есть Script Console, которая работает на Groovy. По сути это объектно-ориентированный язык программирования для платформы Java. Всё, что нам нужно знать, что это работает как консоль, которая выполняет команды на уровне машины. К примеру, вывод команды id

<img width="1416" height="766" alt="Screenshot from 2026-08-27 09-24-28" src="https://github.com/user-attachments/assets/efea4a8a-512d-41ee-978a-1f52ada3aea2" />

По написанию чистая Java. Собственно, вектор понятен, и это опять ревёрс-шелл

<img width="593" height="212" alt="Screenshot from 2026-08-27 09-21-06" src="https://github.com/user-attachments/assets/3959236e-73a7-4157-b928-5aa02c5dd04c" />

<img width="1416" height="766" alt="Screenshot from 2026-08-27 09-27-05" src="https://github.com/user-attachments/assets/5a4ecda1-22c1-4e82-b35e-83de452949e5" />

Команда для ревёрс-шелла есть тут - revshells.com

<img width="742" height="285" alt="Screenshot from 2026-08-27 09-27-10" src="https://github.com/user-attachments/assets/bf366010-dfed-4475-b27a-45256181e97e" />

После изучения окружения повторный вектор с кредами в файле

<img width="1258" height="245" alt="Screenshot from 2026-08-27 09-32-26" src="https://github.com/user-attachments/assets/8911ed9e-f17a-499b-b011-71821418403d" />

Ну и осталось зайти и забрать рут флаг

<a id="flag2" class="anchor"></a>
<img width="855" height="402" alt="Screenshot from 2026-08-27 09-33-30" src="https://github.com/user-attachments/assets/1cd67d07-0a58-4b63-a861-e26480035e45" />

Комната успешно пройдена

## Мнение о комнате

Тот случай, когда две прошлые комнаты были значительно сложнее (а там лёгкая и средняя сложность по оценке площадки), при этом подобная сложная даётся весьма просто. Все векторы стандартные и достаточно очевидные. Сложным точно не назвать, даже не зная особенностей внутри WP и Jenkins, абсолютно несложно найти и изучить. Все креды в открытой форме. Нет CVE или SUID бинарей, комната вышла простой и стандартной. В целом эта комната больше подойдёт под США. В РФ больше развит Bitrix, чем WP. А аналогов Jenkins и того больше. В общем, хорошая комната для отработки стандартной практики. Всё же пароли по сей день слабая часть. Дело даже не в сложности пароля, многие используют один пароль годами, а потом волшебным образом всё взломали. По сути оставили на незащищённом сайте свой единственный пароль. А СУБД хранит пароли в чистом виде. Всё. Все ваши аккаунты обречены. А на этом у нас всё. Удачи
