<img width="1057" height="298" alt="Screenshot from 2026-07-14 13-27-46" src="https://github.com/user-attachments/assets/2b1cf4a0-aa30-47b4-8d04-e0f7a562b8ad" />
Доброго времени суток у нас на прохождение Mr Robot CTF. Mr Robot весьма популярный фильм о хакерах. Долго не томим и идём к делу.


Получив Attacker machine и Lab machine, мы начинаем с самой базовой команды для проверки, с чем мы вообще работаем.
<img width="725" height="207" alt="Screenshot from 2026-07-14 13-30-07" src="https://github.com/user-attachments/assets/b7a0277d-ae8e-4139-b32b-37593b6fa902" />

Стандартный набор портов, идём по стандартному сценираю через веб.


<img width="1196" height="614" alt="Screenshot from 2026-07-14 13-30-22" src="https://github.com/user-attachments/assets/b1de5628-a9f0-4385-b6ae-0b33322a8f38" />

На вебе нас ожидает что-то типо терминала. Начинаем проверку каждой фразы, смотрим всякие ролики, файлы и текст. Ничего особенно не находим и идём искать скрытые директории и файлы.

<img width="918" height="1246" alt="Screenshot from 2026-07-14 13-52-25" src="https://github.com/user-attachments/assets/652340d3-8cdc-4496-88fb-ce5abe51c94e" />
Обращаем внимание, что у нас стоит CMS WordPress, скорее всего мы будем входить в админку, нужно найти креды или иные подсказки. Начинаем проверку остальных найденных файлов

<img width="1195" height="518" alt="Screenshot from 2026-07-14 13-55-14" src="https://github.com/user-attachments/assets/c565a332-2f77-498d-a9c7-464a05d6f8b3" />
<img width="1195" height="518" alt="Screenshot from 2026-07-14 13-55-17" src="https://github.com/user-attachments/assets/b8a899f2-61ae-45cc-bca0-24318e2d43ba" />
<img width="1195" height="518" alt="Screenshot from 2026-07-14 13-55-25" src="https://github.com/user-attachments/assets/4c9082ab-7b8b-4e60-a546-212a39050bd5" />
Видим интересные подсказки. Нам дают 2 файла и мы естественно проверяем их.

<img width="1195" height="518" alt="Screenshot from 2026-07-14 13-55-33" src="https://github.com/user-attachments/assets/572e27e1-5007-4e1c-af96-8e759c2ab8b8" />
Получаем первый флаг в файле key-1-of-3.txt
<img width="954" height="181" alt="Screenshot from 2026-07-14 13-56-00" src="https://github.com/user-attachments/assets/b574b035-8dd8-4f38-84ba-7f6c13a618b2" />
В файле fsocity.dic мы находим целый список кучи разных слов. Вероятно это кастомный список для брутфорса. Будем проверять эту теорию, но сначала нужно проверить файл, т.к это очень большой список чтобы пробовать что-то брутить.
<img width="919" height="475" alt="Screenshot from 2026-07-14 13-59-37" src="https://github.com/user-attachments/assets/07b8aa56-bd45-4c4f-8958-de954436bef5" />
Лёгкими проверками мы узнаём, что в списке fsocity.dic 858к строк. Крайне много. Делаем проверку на дубликаты. Бинго! Не повторяющихся слов всего-то 11к и это уже выглядит более реально под брутфорс. Сохраняем всё в новый файл zenodot.txt и идём исследовать админку
<img width="919" height="475" alt="Screenshot from 2026-07-14 14-00-13" src="https://github.com/user-attachments/assets/69cff2f2-6f47-44eb-a74b-f70b59c72761" />
Вставляя юзер admin и пробуем пароль 12345678 мы видим интересный ответ. Invalid username. А это значит, что с помощью брута мы можем попробовать найти валидный юзернейм.

<img width="919" height="393" alt="Screenshot from 2026-07-14 14-05-50" src="https://github.com/user-attachments/assets/8b0cc2b5-3886-4885-998a-034058141cb8" />
С помощью гидры мы составляем простенький шаблон для проверки на валидного юзера. Этой командой мы заставляем проверять наличие Invalid username при переборе юзеров с заданным паролем 12345678. Тоесть если Invalid username пропадает при каком-то логине, то он будет валидным. К слову отмечу, что wpscan не выдал юзера т.к wp-json/wp/v2/users просто закрыт. Поэтому брут остаётся самым простым вараинтом для проверки.  
