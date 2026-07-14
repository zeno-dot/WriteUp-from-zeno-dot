<img width="1057" height="298" alt="Screenshot from 2026-07-14 13-27-46" src="https://github.com/user-attachments/assets/2b1cf4a0-aa30-47b4-8d04-e0f7a562b8ad" />
Доброго времени суток у нас на прохождение Mr Robot CTF. Mr Robot весьма популярный сериал о хакерах. Долго не томим и идём к делу.


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
С помощью гидры мы составляем простенький шаблон для проверки на валидного юзера. Этой командой мы заставляем проверять наличие Invalid username при переборе юзеров с заданным паролем 12345678. Тоесть если Invalid username пропадает при каком-то логине, то он будет валидным. И мы находим логин - elliot. Так зовут Главного героя сериала. К слову отмечу, что wpscan не выдал юзера т.к wp-json/wp/v2/users просто закрыт. Поэтому брут остаётся самым простым вараинтом для проверки.  
<img width="480" height="500" alt="Screenshot from 2026-07-14 14-06-31" src="https://github.com/user-attachments/assets/49b14fff-4be5-4657-998a-fbbc502af5e7" />
Теперь мы видим, что если подставить логин elliot с паролем 12345678. То пишет не Invalid username, а что пароль не верный. Остаётся сбрутить пароль аналогичным образом.
<img width="924" height="374" alt="Screenshot from 2026-07-14 14-09-18" src="https://github.com/user-attachments/assets/3addd8e0-bcee-408f-840b-ac68f8c4224e" />
И мы находим пароль. Пароль тоже является отсылкой к сериалу. Пробуем войти в админку
<img width="1199" height="741" alt="Screenshot from 2026-07-14 14-09-49" src="https://github.com/user-attachments/assets/f5197d61-1230-49d6-85cc-b4edf581845a" />
И мы внутри. Начинаем осматриваться чтобы понять вектор.
<img width="1199" height="741" alt="Screenshot from 2026-07-14 14-10-06" src="https://github.com/user-attachments/assets/d3375ffd-f87d-4a57-bd7f-3b2797779c28" />
Элиот уже админ, а значит права у нас самые высокие и не нужно искать способа стать админом.
<img width="1199" height="1085" alt="Screenshot from 2026-07-14 14-12-13" src="https://github.com/user-attachments/assets/297366db-20c0-4b3e-a195-1e65a036f2b2" />
Тут я натыкаюсь на editor форму файлов которые уже есть на сервере. Проверяю действительно ли этот файл существует и соответсвует написанному.
<img width="1199" height="1085" alt="Screenshot from 2026-07-14 14-12-17" src="https://github.com/user-attachments/assets/6e123da7-6177-423b-9958-931de8419566" />
Текст сходится. Значит мы можем изменить php файл на что угодно. Поэтому мы будет пробрасывать ревёрс шел и заходить на тачку. Воспользуемся pentestmonkey и его готовым вариантом ревёрс шела.
<img width="1199" height="1085" alt="Screenshot from 2026-07-14 14-15-48" src="https://github.com/user-attachments/assets/3224bb78-347e-41ad-9735-8ea25c28811e" />
Указываем айпи и порт в файле и нажимаем Update File
<img width="925" height="612" alt="Screenshot from 2026-07-14 14-16-29" src="https://github.com/user-attachments/assets/030e2a23-d043-4e0e-83ab-bd127336a3d3" />
Поднимаем nc на порту 6767
<img width="1201" height="1088" alt="Screenshot from 2026-07-14 14-16-43" src="https://github.com/user-attachments/assets/cc0c29f6-20d0-4199-ac4e-b2fdb28f1ecc" />
Заходим на изменённый файл и видим белый фон, а значит изменение файла точно было. Смотрим в nc
<img width="922" height="607" alt="Screenshot from 2026-07-14 14-16-57" src="https://github.com/user-attachments/assets/6ee0d683-5714-4c2d-b4e1-8f7933e0d16c" />
И мы внутри
<img width="922" height="607" alt="Screenshot from 2026-07-14 14-17-48" src="https://github.com/user-attachments/assets/1a89a279-4eb7-44cb-916a-0d47d7fc3b01" />
Первым делом поднимаем оболочку и начинаем исследовать. Вспоминаем что первый флаг называется key-1-of-3.txt, второй вероятно будет key-2-of-3.txt
<img width="922" height="281" alt="Screenshot from 2026-07-14 14-20-22" src="https://github.com/user-attachments/assets/0d568785-c6e0-412e-bd20-f18ba163234b" />
Ищем этот файл и успешно находим расположение. Заходим в директорию /home/robot и видим 2 файла. key-2-of-3.txt настроен на чтение только пользователем robot которым мы не являемся, а значит и читать смысла нет. Но мы видим второй файл который намекает на дехэш чтобы узнать пароль юзера robot
<img width="902" height="753" alt="Screenshot from 2026-07-14 14-23-06" src="https://github.com/user-attachments/assets/bf4759e7-b4a7-43a1-9c31-53284e4887c5" />
Сохраняем хэш в файле hash.txt и пробуем дехэш на списке который давали нам в самом начале, а точнее его очищенная версия zenodot.txt
<img width="902" height="896" alt="Screenshot from 2026-07-14 14-24-33" src="https://github.com/user-attachments/assets/b5d3eb4a-1117-4962-9560-befea9313d54" />
Дехэш неуспешен, пароль не найден. Пробуем словарь rockyou.txt
<img width="902" height="1113" alt="Screenshot from 2026-07-14 14-25-13" src="https://github.com/user-attachments/assets/b0a18b69-d700-4699-ad8b-585f4121b163" />
пароль успешно найден и это просто английский алфавит по порядку. Теперь заходим, как пользователь robot
<img width="574" height="134" alt="Screenshot from 2026-07-14 14-26-45" src="https://github.com/user-attachments/assets/0e5d2bf5-af2e-41e9-9a0b-d914b4d6d907" />
всё проходит успешно и остаётся только достать 2 флаг
<img width="574" height="216" alt="Screenshot from 2026-07-14 14-27-43" src="https://github.com/user-attachments/assets/eb0279a9-aa96-4d7d-ba5f-156f4438a451" />
Второй флаг мы успешно получили. Теперь нужно найти 3 флаг
<img width="789" height="491" alt="Screenshot from 2026-07-14 14-30-33" src="https://github.com/user-attachments/assets/d90acdf4-c55b-4ab4-979e-ca22553c30c2" />
find нам не помогает. Либо название другое, либо директория закрыта. Вероятнее всего нам нужен root. sudo прав у robot не имеется. Ищем суид бинари. И натыкаемся на nmap, явно нам давая сигнал о веротяной рут эскалации.

<img width="930" height="566" alt="Screenshot from 2026-07-14 14-31-46" src="https://github.com/user-attachments/assets/d11f3bd8-e24d-432a-a5c2-44cceea381b0" />
Пользуясь gtfobins находим nmap и пробуем зайти от рута. Предложенный вариант оказывается не совсем корректный и !/bin/sh не даёт оболочку, а вот !sh дал т.к синтаксически интерпретатор nmap просто не увидил данный путь. Но за счёт PATH вариант с !sh сработал корректно.
<img width="904" height="487" alt="Screenshot from 2026-07-14 14-33-46" src="https://github.com/user-attachments/assets/7e043f3d-27e5-4a81-9424-060859dc1c14" />
Заходим в папку рута и спокойно читаем последний флаг



<img width="1081" height="453" alt="Screenshot from 2026-07-14 14-34-21" src="https://github.com/user-attachments/assets/c945fc06-1c2c-408d-81e3-dac0a52c5188" />



Мнение о комнате

В целом комната вышла интересная. Хотя порой не понятно, как THM определяет сложность и время прохождения для комнаты. Пройти за пол часа можно только в случае если знаешь куда тыкать, а пока изучишь весь материал в вебе и посмотришь все ролики, уже может пройти пол часа. Но это больше лирическое отступление. Команта хорошо показывает, что готовые решения это не абсолют безопасности WP является одним из самых популярных CMS всего мира, но даже там есть ряд проблем и уязвимостей. Для чего сделали админку такой болтливой не понятно. Да и в целом админка это чаще всего дополнительный вектор для атаки т.к могут сбрутить или выловить креды админа. Злоумышленник заходит в админку и может спокойно пролить php шел, ревёрс шел. Да хоть снести всё. Админка должна быть урезанная и иметь 2фа с подтверждением хотя бы через мессенджер, а лучше кодом на телефон чтобы сразу видеть потенциальные атаки. В остальном команта весьма базовая и не менее интерсеная особенно для тех, кто смотрел сериал.









