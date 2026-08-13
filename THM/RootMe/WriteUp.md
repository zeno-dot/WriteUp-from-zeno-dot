---
title: "RootMe — TryHackMe"
---

# RootMe — TryHackMe

<img width="1923" height="277" alt="Screenshot from 2026-08-13 12-06-41" src="https://github.com/user-attachments/assets/6a6d4254-009b-4f79-b726-47aeed6ecfc3" />

<div class="wu-meta">
<span>Сложность <b>Лёгкая</b></span>
<span>Платформа <b>TryHackMe</b></span>
</div>

<div class="wu-flags">
<a href="#flag1">🚩 Флаг 1</a>
<a href="#flag2">🚩 Флаг 2</a>
</div>

Доброго времени суток, на прохождении очередная лёгкая комната. Особо говорить нечего, к делу.

## Разведка

Получив Lab machine, мы начинаем с самой базовой команды для проверки, с чем мы вообще работаем.

<img width="929" height="387" alt="Screenshot from 2026-08-13 12-48-34" src="https://github.com/user-attachments/assets/e60d8a91-ab4d-4eec-8ac5-ec98471fed1f" />

И мы сразу можем ответить на ряд вопросов, поставленных комнатой.
"Scan the machine, how many ports are open?" — очевидно, что ответ "2".
"What version of Apache is running?" — не менее очевидный ответ "2.4.41".
И вопрос "What service is running on port 22?", где и без nmap можно угадать, что ответ "ssh".

Дальше "Find directories on the web server using the GoBuster tool." Этим и займёмся.

<img width="1072" height="738" alt="Screenshot from 2026-08-13 12-51-55" src="https://github.com/user-attachments/assets/a09560b8-af61-412b-954d-e15f2eca7de5" />

У нас 2 директории. Но ответ на "What is the hidden directory?" будет "/panel/".

## Веб и reverse shell

Дальше изучаем веб.

<img width="1150" height="1227" alt="Screenshot from 2026-08-13 12-52-22" src="https://github.com/user-attachments/assets/888a57ef-f782-4538-9d85-c18edb02afa2" />
<img width="1150" height="1227" alt="Screenshot from 2026-08-13 12-52-28" src="https://github.com/user-attachments/assets/ab946bec-3bff-46f1-b972-071c70ede46e" />

Мы видим точку для заливки ревёрс-шелла и uploads, где, вероятно, и будут все загруженные файлы. Пробуем залить ревёрс-шелл, который выглядит так:

<img width="1002" height="1198" alt="Screenshot from 2026-08-13 13-53-50" src="https://github.com/user-attachments/assets/0af7e9b1-ec3b-49fa-8cdf-49a7ecacd729" />

То есть pentestmonkey.

<img width="569" height="149" alt="Screenshot from 2026-08-13 13-02-19" src="https://github.com/user-attachments/assets/d597fb69-08df-43c9-b687-060ac3665b64" />

Ставим слушателя и пробуем заливать ревёрс-шелл.

<img width="1150" height="1237" alt="Screenshot from 2026-08-13 13-03-10" src="https://github.com/user-attachments/assets/dae3d983-54b5-403d-8c73-8a75fdb73139" />
<img width="1150" height="1237" alt="Screenshot from 2026-08-13 13-03-19" src="https://github.com/user-attachments/assets/14bfd2e2-53b2-4663-bf10-590ffa73b816" />

Скорее всего, файлы типа .php стоят в блоке. Попробуем немного поменять формат, к примеру на .php5.

<img width="1150" height="1237" alt="Screenshot from 2026-08-13 13-26-17" src="https://github.com/user-attachments/assets/95bf317f-9c4c-418d-b206-84266c7380fd" />

И всё успешно получается, переходим по пути, где лежит файл, и смотрим слушателя.

<img width="1185" height="306" alt="Screenshot from 2026-08-13 13-26-23" src="https://github.com/user-attachments/assets/8c6ca0ad-86bc-4f6b-9b72-8cab5774c6ef" />

Поднимаем оболочку.

<img width="1185" height="306" alt="Screenshot from 2026-08-13 13-27-45" src="https://github.com/user-attachments/assets/0eb4584d-66c7-46a7-abb2-2ca82dffa341" />

Ищем первый флаг. Для этого нужно найти домашнюю директорию.

<img width="1185" height="866" alt="Screenshot from 2026-08-13 13-30-12" src="https://github.com/user-attachments/assets/2c9ed9f9-7d44-4b0a-8465-4ef7be8b3699" />

Стандартный путь /var/www открываем и изучаем.

<a id="flag1" class="anchor"></a>
<img width="1185" height="309" alt="Screenshot from 2026-08-13 13-30-41" src="https://github.com/user-attachments/assets/048cea21-bb57-48a5-8d5d-4589d712a8be" />

## Повышение привилегий

Первый флаг получен. Дальше у нас стоит вопрос по SUID. Начинаем поиск.

<img width="1185" height="418" alt="Screenshot from 2026-08-13 13-32-56" src="https://github.com/user-attachments/assets/601e7beb-5e72-4304-9ad1-147f9d3703b7" />

Самый удобный формат для эскалации. Питон. Поэтому ответ для "Search for files with SUID permission, which file is weird?" будет "/usr/bin/python".

Поднимаем оболочку с возможностями рута через питон.

<img width="1185" height="418" alt="Screenshot from 2026-08-13 13-33-39" src="https://github.com/user-attachments/assets/991eea12-fd5a-4f2a-a460-7abb9a5dc747" />

Ну и забираем флаг.

<a id="flag2" class="anchor"></a>
<img width="1185" height="383" alt="Screenshot from 2026-08-13 13-34-38" src="https://github.com/user-attachments/assets/9ea4fd57-1016-426b-bce3-5af0ed06a7d2" />

## Мнение о комнате

В целом комната базовая база. Найти в вебе вариант залива шела, получится доступ к машине и провести эскалацию. Для новичков очень хорошо подойдет, т.к. залив шела — это чаще всего про обход защит, а не просто загрузить на сервер файл или поменять файл темы в CMS.
