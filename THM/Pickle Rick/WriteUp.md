# Pickle Rick — TryHackMe
<img width="512" height="512" alt="image" src="https://github.com/user-attachments/assets/0d756119-aa8b-46fd-ac86-e02eb87f38ad" />

**Сложность:** Лёгкая  
**Платформа:** TryHackMe  
<br>
<br>
<img width="1844" height="1004" alt="Screenshot from 2026-06-26 10-33-48" src="https://github.com/user-attachments/assets/8e1e9602-a40c-4de6-ade5-fd04872890a0" />
Доброго времени суток. У нас на прохождении Огурчик Рик. Одна из самых популярных комнат на THM, да и в целом в мире. Давайте начнём разбор
<br>
Получив Attacker machine и Lab machine мы начинаем с самой базовой команды для проверки с чем мы вообще работаем
<br>
<br>
<img width="723" height="200" alt="Screenshot from 2026-06-26 10-34-58" src="https://github.com/user-attachments/assets/8ddec3dd-b289-45c5-80ca-231e0c0b485f" />
<br>

Видя 80 порт и 22, начнём именно с изучения http
<br>
<br>
<img width="1520" height="773" alt="Screenshot from 2026-06-26 10-35-34" src="https://github.com/user-attachments/assets/c295094f-faec-404d-9028-804fb71fc4d7" />
Обычный статичный веб без каких-то ссылок, значит будем пробовать перебирать скрытые от глаза директории и файлы
<br>
<br>
<img width="1067" height="637" alt="Screenshot from 2026-06-26 11-03-30" src="https://github.com/user-attachments/assets/96ca313f-ed97-4d33-9d53-8fa4a611bf9c" />
Нашлось не так много, но всё требует изучения
<br>
<br>
<img width="1117" height="528" alt="Screenshot from 2026-06-26 10-49-58" src="https://github.com/user-attachments/assets/98a70c63-19f0-4175-8ae1-41b4f2db047a" />
В assets кучу внутренних файлов, но главное на что нужно обратить внимание это количество фото на сайте. Мы видели лишь одну в начале, а на сайте их целых 4. Значит вектор поиска новых директорий явно имеет смысл
<br>
<br>
<img width="1519" height="1119" alt="Screenshot from 2026-06-26 10-50-23" src="https://github.com/user-attachments/assets/8bea8ec6-292c-4b32-ba82-22a6e882f309" />
Смотрим следующие файлы
<br>
<br>
<img width="1519" height="1119" alt="Screenshot from 2026-06-26 10-50-40" src="https://github.com/user-attachments/assets/157be0c6-43fa-4da9-9aca-45f019a4f410" />
Запоминаем странную строку, возможно она оставлена специально для какого-то продвижения.
<br>
<br>
<img width="1519" height="1264" alt="Screenshot from 2026-06-26 10-59-06" src="https://github.com/user-attachments/assets/83e5a377-4e86-4f1d-983d-650f0433c5ed" />
А вот и логин панель. У нас есть в robot.txt странная строка, возможно это логин или пароль, а может и вовсе логин и пароль одновременно. При проверке гипотезы о том, что это и логин и пароль мы ловим неудачу и продолжаем исследовать сайт дальше. В файле clue.txt находится подсказка для будущего флага поэтому опустим его и будем изучать сайт дальше.
<br>
<br>
<img width="1519" height="1264" alt="Screenshot from 2026-06-26 10-51-14" src="https://github.com/user-attachments/assets/a0c95635-ee6c-4407-a0ec-9605c2714073" />
В ходе поиска на главной странице был найден юзернейм пользователя. В целом задумка имеет место быть т.к стоит уметь анализировать код и структуру сайта. Имея юзер, можем предположить что пароль находится в robots.txt
<br>
<br>
<img width="1519" height="1264" alt="Screenshot from 2026-06-26 11-00-25" src="https://github.com/user-attachments/assets/125b36d5-a196-4c85-8b99-c553c1063f71" />
И мы зашли. Command Panel уже звучит как что-то исполняемое, начинаем проверять
<br>
<br>
<img width="1519" height="1264" alt="Screenshot from 2026-06-26 11-00-37" src="https://github.com/user-attachments/assets/a6e35ea4-7af4-4f94-9a84-5636ebf55da2" />
whoami дал результат, а значит у нас прямая панель для использовании комнад на сервере от имени www-data. Начинаем осматриваться и проверять где мы. Команда ls -la вывела нам 2 txt файла которые начинаем читать. Хоп и команда cat не работает, чтение заблокировано? Проверим другие способы. Команда more тоже не работает. Команда less - успех. Чтение файла одобрено и мы получаем первый флаг. Но можно было сделать всё чуть-чуть проще.
<br>
<br>
<img width="918" height="259" alt="Screenshot from 2026-06-26 11-04-36" src="https://github.com/user-attachments/assets/da5d1ec4-1934-49c7-96c5-958984053fc2" />
<br>
Мы могли просто вписать в URL сам флаг и получить его, а файл clue.txt намекает на то, что 2 флаг где-то в системе
<br>
<br>
<img width="1513" height="576" alt="Screenshot from 2026-06-26 11-04-53" src="https://github.com/user-attachments/assets/9040df05-d80d-4b79-b224-3f661349d0cd" />
Команда pwd помогает нам понять из какой директории мы будем использовать команды. Чтобы изучить сервер мы будем использовать ls -la /... и таким образом передвигаться по системе.
<br>
<br>
<img width="1513" height="1005" alt="Screenshot from 2026-06-26 11-09-44" src="https://github.com/user-attachments/assets/8b130d5d-545c-4429-ba48-150951db1587" />
В корневой папке (ls -la /) мы видим домашнюю папку home. Так как это папка хранит в себе данные стандартных пользователей её обязательно нужно изучить.
<br>
<br>
<img width="1513" height="1005" alt="Screenshot from 2026-06-26 11-09-49" src="https://github.com/user-attachments/assets/da245e64-b626-4dc1-a880-45b5edc9cd4b" />
ls -la /home даёт нам увидеть 2 пользователей. Так как у нас команта про Рика, то и начнём изучать его папку

<br>
<br>
<img width="1513" height="1005" alt="Screenshot from 2026-06-26 11-12-22" src="https://github.com/user-attachments/assets/0d51e77a-7693-452a-a145-d4a2827aba2f" />
И мы находим 2 флаг. Но есть небольшой интересный нюанс
<br>
<br>
<img width="1513" height="1005" alt="Screenshot from 2026-06-26 11-13-02" src="https://github.com/user-attachments/assets/64f52f2c-1e8a-45b0-ac23-ffe7fd0efdf2" />
Из-за пробела в названии файла, команда проходила не корректно и не давала нам вывод
<br>
<br>
<img width="1513" height="1005" alt="Screenshot from 2026-06-26 11-13-23" src="https://github.com/user-attachments/assets/62fed95f-9c9e-43eb-8814-5016c7317d5d" />
Данная команда заставляет вывести все возможные файлы в данной директории. А так как у нас там один файл, то и вывод является флагом. Но не стоим на месте. У нас ещё 3 флаг. Скорее всего данный флаг в папке рут, но нужно проверить сначала директорию пользователя ubuntu, как возможный вектор входа в машину
<br>
<br>
<img width="1513" height="1005" alt="Screenshot from 2026-06-26 11-13-53" src="https://github.com/user-attachments/assets/0f9c2f5e-ccd2-46b2-85fe-3c532607e5b3" />
Читаем все файлы и ищем зацепку
<br>
<br>
<img width="1513" height="1005" alt="Screenshot from 2026-06-26 11-16-30" src="https://github.com/user-attachments/assets/5a4568d7-34fc-4299-9906-8bede60ea647" />
Важных или полезных данных не заметил. Поэтому решаю пробросить ревёрс шел и уже полноценно копаться в машине в поисках повышений привелегий, тоесть войти в пользователя root
<br>
<br>
<img width="760" height="252" alt="Screenshot from 2026-06-26 11-20-39" src="https://github.com/user-attachments/assets/482dcfb0-b1e7-44f5-9bcb-3b854c60b320" />
<br>
Поднимаю nc на прослушку 4444 порта. Пробую стандартные варианты проброса ревёрс шела через баш, но не пускает. Пробую вариант с питоном
<br>
<br>
<img width="1523" height="363" alt="Screenshot from 2026-06-26 11-27-47" src="https://github.com/user-attachments/assets/43785aa1-160f-468f-8b46-2835b20ad809" />
Сайт начинает долго грузить и смотрим в терминал
<br>
<br>
<img width="1091" height="796" alt="Screenshot from 2026-06-26 11-27-55" src="https://github.com/user-attachments/assets/b186eb75-17d9-4748-97e7-47d733bab64a" />
Ревёрс шел успешно проброшен
<br>
<br>
<img width="1091" height="796" alt="Screenshot from 2026-06-26 11-28-40" src="https://github.com/user-attachments/assets/4cea5522-3f4b-4f7b-85fe-1dde6bcfa6d3" />
Поднимаем оболочку TTY для полноценного шела и начинаем искать способ эскалации
<br>
<br>
<img width="766" height="184" alt="Screenshot from 2026-06-26 11-40-15" src="https://github.com/user-attachments/assets/80d881c3-b8e0-4c3c-bce6-e0679b6bc0fa" />
<br>
Первым делом проверяем sudo -l чтобы понять вектор. И мы видим что пользователь www-data может выполнять команды от имени root без пароля. А это значит что получить оболочку от рута не составит проблем.
<br>
<br>
<img width="766" height="184" alt="Screenshot from 2026-06-26 11-40-15" src="https://github.com/user-attachments/assets/b67aef36-2700-4bb0-8aba-30b90976ac45" />
<br>
И вот мы уже рут без всяких проблем. Теперь наша задача проверить папку рута на предмет флагов
<br>
<br>
<img width="775" height="491" alt="Screenshot from 2026-06-26 11-40-49" src="https://github.com/user-attachments/assets/0fa2cfb3-746f-44df-93d7-4bc1b3b047bb" />
<br>
И вот мы забираем последний 3 флаг.
<br>
<br>
<br>
<br>




# Мнение о комнате
<br>
В целом комната даже проще чем думал изначально. Это прям CTF в базовом понимании. Что-то там потыкать, перебрать, потыкать и обрадываться. Найти юзер через код элемента в современных реалиях пожалуй уже на грани невозможного, ровно как и пароль в robots.txt в открытом виде. В итоге из-за того, что нужно было думать более приземлённо не обязательно было делать ревёрс шел. Судо можно было выполнить в целом и из панели. Тоесть прохождение было буквально в том чтобы залогиниться и выполнить 3 успешные команды. В общем и команте уже за 2200 дней. Такая комната подойдёт грубо говоря даже самым маленьким. Из знаний достаточно nmap, gobuster или его аналог + знания в работе терминала линукса. Так что в целом комнта очень хорошо подойдёт тем, кто хочет потрогать в первые что такое пентест и решение CTF. 





<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>














