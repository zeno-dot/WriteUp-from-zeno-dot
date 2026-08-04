---
title: "Basic Pentesting — TryHackMe"
---

# Basic Pentesting — TryHackMe

<img width="1626" height="265" alt="Basic Pentesting" src="https://github.com/user-attachments/assets/461b434d-c13b-4d4c-8857-6a7cb8e63ce2" />

<div class="wu-meta">
<span>Сложность <b>Лёгкая</b></span>
<span>Платформа <b>TryHackMe</b></span>
</div>

Доброго времени суток на прохождении Basic Pentesting. Очередная популярная комната, поэтому без лишних слов идём к делу.

## Разведка

Получив Attacker machine и Lab machine, мы начинаем с самой базовой команды для проверки, с чем мы вообще работаем.

<img width="808" height="329" alt="Результат сканирования nmap" src="https://github.com/user-attachments/assets/9b9ddcd7-8d43-4386-89c6-ef844d441cf0" />

Видим http и smb. Скорее всего сам вектор будет развиваться именно через smb, изучаем веб через gobuster.

## Веб

<img width="1005" height="660" alt="Перебор директорий gobuster" src="https://github.com/user-attachments/assets/79938ccd-dbfe-484f-a9bf-471c6d911f29" />

Видим index.html и development. Изучаем

<img width="1195" height="488" alt="Каталог development" src="https://github.com/user-attachments/assets/01856c80-cd47-4594-aee0-1f427f06cc3a" />

Смотрим оба файла

<img width="1195" height="488" alt="Содержимое dev.txt" src="https://github.com/user-attachments/assets/bb4e652d-6e5f-48fe-aecb-3e6eaf77238b" />
<img width="1195" height="488" alt="Содержимое j.txt" src="https://github.com/user-attachments/assets/7f3147c4-6829-45d5-846f-4019aa28ba0d" />

По полученным текстам какого-то глобального продвижения нет. Понимаем, что есть внутренние проблемы безопасности и 2 неизвестных пользователя. Проверим вектор smb

## SMB

<img width="809" height="617" alt="Перечисление SMB и файл staff.txt" src="https://github.com/user-attachments/assets/00b0d0b2-c44a-46c0-9f06-0e4c0bf9989c" />

В целом ничего важного мы не узнали, кроме наличия 2 пользователей: Jan и Kay. На THM в поле ответа "What is the username?" используем оба имени и понимаем, что нас интересует именно Jan. Пробуем брутить ssh

## Перебор SSH

<img width="810" height="481" alt="Подбор пароля через Hydra" src="https://github.com/user-attachments/assets/cee36a77-2f69-4dff-a2c2-10eda8d34ce6" />

Пароль успешно был подобран, входим в машину

<img width="923" height="883" alt="Вход по SSH как jan" src="https://github.com/user-attachments/assets/c12ca540-509f-4856-b5ea-fa99987f74bd" />

## Повышение привилегий

Как обычно нас интересует вектор повышения привилегий. В данном случае нам нужен пользователь Kay

<img width="923" height="883" alt="Проверка sudo и SUID" src="https://github.com/user-attachments/assets/c0b47ddd-71b5-4075-8ea6-ff899926064b" />

У Jan нет прав судо и не видно необычных бинарей, ищем дальше

<img width="923" height="883" alt="Проверка доступа к /etc/shadow" src="https://github.com/user-attachments/assets/2fa48a9a-bb19-4384-ba77-56127df04bb7" />

В сообщениях писали про /etc/shadow, но чтение недоступно, продолжаем

<img width="923" height="883" alt="Домашняя директория Kay с читаемым id_rsa" src="https://github.com/user-attachments/assets/65826b74-63b5-4dc6-8420-d0892818714c" />

В директории Kay мы видим открытый id_rsa. Пробуем залогиниться не имея пароля через passphrase. Если коротко, то passphrase — это защита от кражи (пароль от самого файла, не пароль пользователя)

<img width="817" height="328" alt="Снятие хэша ключа через ssh2john" src="https://github.com/user-attachments/assets/2b70e303-fa39-4556-aa65-731b4af61c95" />
<img width="806" height="363" alt="Подбор passphrase через john" src="https://github.com/user-attachments/assets/0c3e5835-1c43-4444-af3d-03797967c727" />

Фраза успешно получена, и мы можем зайти в качестве пользователя Kay. Вектор passphrase оказался верным, важное уточнение в качестве доп информации. Подобные ключи должны иметь права 600 и владельца. Тогда не получилось бы даже файл забрать.

<img width="816" height="818" alt="Вход по SSH как Kay" src="https://github.com/user-attachments/assets/d97dff32-ab0a-4a3e-8a7a-c6f77abd1ad5" />

И получаем последний ответ на последний вопрос "What is the final password you obtain?"

<img width="816" height="309" alt="Финальный пароль в pass.bak" src="https://github.com/user-attachments/assets/e98494c8-8561-4d07-b2eb-41b9313b7b55" />

## Мнение о комнате

Комната простая и скорее всего из-за этого и весьма популярна. По сути своей вся задача свелась к бруту ssh и бруту passphrase. Даже по системе особо не пришлось лазить. Не говоря о том, что даже думать не нужно, куда лезть и смотреть, т.к. ответы по комнате сами ведут по нужному пути. Даже для новичков такое посоветовать не получится, т.к. банально не развивает логику и мышление о подходе. Пока ищешь ответ, изучаешь то, что тебе не поможет в данном случае, но поможет в других, т.к. изучал ранее. Такое можно в школе давать в качестве интересного квеста.
