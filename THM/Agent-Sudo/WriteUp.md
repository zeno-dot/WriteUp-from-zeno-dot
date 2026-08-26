---
title: "Agent Sudo — TryHackMe"
---

# Agent Sudo — TryHackMe

<img width="1146" height="236" alt="Screenshot from 2026-08-26 09-40-29" src="https://github.com/user-attachments/assets/48b39975-ee15-47f0-87eb-41106655872c" />

<div class="wu-meta">
<span>Сложность <b>Лёгкая</b></span>
<span>Платформа <b>TryHackMe</b></span>
</div>

<div class="wu-flags">
<a href="#flag1">🚩 Флаг 1 (user)</a>
<a href="#flag2">🚩 Флаг 2 (root)</a>
</div>

Доброго времени суток, на прохождении лёгкая комната Agent Sudo. Комната вышла весьма специфической и попыталась запихнуть в себя всё что знал создатель (по крайней мере по ощущениям). Порой это выглядит действительно странно. Даются очень расплывчатые подсказки на раскрутку которых может уйти и час. В общем начинаем

## Разведка

Получив Lab machine, мы начинаем с самой базовой команды для проверки, с чем мы вообще работаем.

<img width="935" height="410" alt="Screenshot from 2026-08-26 09-46-24" src="https://github.com/user-attachments/assets/f22d5f67-4b46-4fe0-8b9f-9a491182fb8f" />

Стандартное развитие через веб, смотрим и изучаем.

<img width="1039" height="369" alt="Screenshot from 2026-08-26 09-47-01" src="https://github.com/user-attachments/assets/39dfa07e-d818-4d46-a3f2-0832650e17f2" />

Дают подсказку про User-Agent (UA). Но сначала проверим gobuster на скрытые директории

<img width="1216" height="671" alt="Screenshot from 2026-08-26 09-48-56" src="https://github.com/user-attachments/assets/8c57ee64-07b1-4589-9018-f93408d5b5bc" />

Ничего не нашлось, пробуем раскрутку по UA.

<img width="882" height="382" alt="Screenshot from 2026-08-26 09-47-30" src="https://github.com/user-attachments/assets/653ad4e0-a13d-4930-a3ff-55088b65dea5" />

В ходе перебора различных значений поймал первый вывод, который ясно даёт понять что путь выбран верно и осталось найти верный триггер. На самом деле я перебрал кучу вариантов и форматов. Даже брут словари использовал. На деле оказалось всё чуть проще

<img width="1205" height="343" alt="Screenshot from 2026-08-26 09-47-53" src="https://github.com/user-attachments/assets/bf793382-f5a2-4e33-a6d7-09c3c8c51220" />

В самом то деле, в UA нужно было подставить C. Не совсем понятно почему это должно быть прям логичным. К примеру если бы написали про 26 пользователей, у меня бы сработал триггер на английский алфавит и я бы побыстрее додумался на перебор по буквам. 25 выглядит как случайное число которое ничего не даёт. Да и в целом вариант с A или B не подходил, а с С уже да. Очень странное решение, но мы идём дальше. Имея предположительно логин chris, пробуем найти к нему пароль путем брутфорса ФТП (В очередной раз вопросы на THM чётко дают понять какой вектор дальше)

<img width="1310" height="383" alt="Screenshot from 2026-08-26 09-49-15" src="https://github.com/user-attachments/assets/735e7dfe-497c-424b-8149-2fc4a25fc39b" />

Брут успешно выполнен, смотрим что он нам может предложить

<img width="1310" height="700" alt="Screenshot from 2026-08-26 09-50-56" src="https://github.com/user-attachments/assets/1d1cd55d-730d-4669-b31d-cc99703c0834" />

<img width="1310" height="221" alt="Screenshot from 2026-08-26 09-51-16" src="https://github.com/user-attachments/assets/70832b1d-1857-44cb-8a9d-6f12679666d3" />

И тут новые приколы. Теперь у нас изучение скрытых посланий в картинках, то есть стеганография. Давайте изучать полученные картинки

<img width="1310" height="474" alt="Screenshot from 2026-08-26 09-51-53" src="https://github.com/user-attachments/assets/463323af-d1df-4d7f-bb1b-dcb0d9a04159" />

В файле cutie.png найден вшитый zip файл, достаём его и смотрим что имеем

<img width="1310" height="474" alt="Screenshot from 2026-08-26 09-53-16" src="https://github.com/user-attachments/assets/245fde9c-076f-42a5-b41c-bfa941ebb84d" />

У нас запароленный архив, пробуем брутить

<img width="1310" height="474" alt="Screenshot from 2026-08-26 09-58-48" src="https://github.com/user-attachments/assets/560d0816-bc0f-497c-81f3-f27b0d88df94" />

И смотрим что внутри

<img width="614" height="234" alt="Screenshot from 2026-08-26 09-59-24" src="https://github.com/user-attachments/assets/adfc0adf-4dbe-412e-b2f3-6e345c225bdb" />

<img width="566" height="66" alt="Screenshot from 2026-08-26 10-00-13" src="https://github.com/user-attachments/assets/facf6849-a723-40e0-9f1f-c215c26ddba3" />

В целом нам дали подсказку, но очень странного формата. Отправить куда-то фотографию. В ходе небольших рассуждений, пришел к такому варианту

<img width="797" height="203" alt="Screenshot from 2026-08-26 10-14-53" src="https://github.com/user-attachments/assets/b56e383c-6bf7-4c61-b49c-9dac04dd29f3" />

Чтож, теперь мы имеем и логин и пароль. Дальше идёт подключение по SSH. В обзоре это выглядит возможно быстро и просто. Но на деле имея минимум инфы приходиться проверять все гипотезы которых бывает крайне много и уже на этот момент ушло порядка 2 часов. Даже если посмотреть как проходят другие люди по времени это в среднем от 2-5 часов. При заявленном времени 45 минут. Проблема не в сложности задачи, а в том что нужно проявить безумие, а не креатив. Догадаться нужно было почти на каждом шагу, при том что подсказки особо ничего и не дают. Идём дальше

<img width="944" height="429" alt="Screenshot from 2026-08-26 10-16-06" src="https://github.com/user-attachments/assets/65c0fae5-022b-4a5b-ad21-349745a2ed17" />

Успешный вход, забираем флаг

<img width="944" height="429" alt="Screenshot from 2026-08-26 10-16-25" src="https://github.com/user-attachments/assets/148c831c-e1fe-4af0-bdb3-15f7f46be3fa" />

<a id="flag1" class="anchor"></a>

Дальше у нас есть фотка и вопрос от THM "What is the incident of the photo called?" То есть название инцидента

<img width="1000" height="300" alt="Alien_autospy" src="https://github.com/user-attachments/assets/2832ff85-ebdb-4c82-913d-74b4fcd14358" />

Пробуем поиск

<img width="1256" height="838" alt="Screenshot from 2026-08-26 10-22-53" src="https://github.com/user-attachments/assets/64699c42-d459-4167-9669-5a25223aea70" />

На самом деле вариантов было много как это называется, пришлось добрых 5 минут сделать поиск чисто по англоязычной тематике и искать все варианты названия. В итоге ответ "Roswell alien autopsy" — хотя опять же есть иные варианты записи, не понятно для чего вообще эта задача была т.к она абсолютно не обязательная. Дальше у нас эскалация до рута

<img width="1103" height="111" alt="Screenshot from 2026-08-26 10-35-45" src="https://github.com/user-attachments/assets/d0671622-79c3-451e-864e-0ebbdcb3e46a" />

Вектор найден и это CVE-2019-14287, простейшая эксплуатация

<img width="1103" height="86" alt="Screenshot from 2026-08-26 10-35-56" src="https://github.com/user-attachments/assets/f455674d-95cb-4be3-a1df-b0409a16462d" />

И забираем рут флаг

<img width="1103" height="417" alt="Screenshot from 2026-08-26 10-36-30" src="https://github.com/user-attachments/assets/57068ebf-30df-4178-8783-49d3ea213375" />

<a id="flag2" class="anchor"></a>

И всё. Часть внутри тачки настолько простая, что это даже обидно

## Мнение о комнате

Комната очень специфическая. Проверка кучи вариантов и почти ручного брутфорса. Подсказки порой только мешают и без них будто даже проще было. Но вышло обширно по инструментам. Проблема в том что это сборная солянка из всего подряд. Прыгать с одной ветки на другую. Стеганография, перебор пароля у всего подряд и резко осинт ещё. И даже вопрос в конце "(Bonus) Who is Agent R?" для чего не понятно. Оно не выглядит как цельная комната, мутант какой-то. В общем моё недовольство и так видно, лучше всего такую комнату читать, чем проходить самому
