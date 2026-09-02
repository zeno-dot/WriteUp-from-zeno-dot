---
title "Crocc Crew - TryHackMe"
---

# Crocc Crew - TryHackMe

<img width="1735" height="243" alt="Screenshot from 2026-09-02 12-45-56" src="https//github.com/user-attachments/assets/1d998a7b-61c6-4241-8905-bacdd40af1c5" />

<div class="wu-meta">
<span>Сложность <b>insane / безумная</b></span>
<span>Платформа <b>TryHackMe</b></span>
</div>

<div class="wu-flags">
<a href="#flag1">🚩 User flag</a>
<a href="#flag2">🚩 Privileged User's flag</a>
<a href="#flag3">🚩 Second Privileged User's flag</a>
<a href="#flag4">🚩 Root flag</a>
</div>

Доброго времени суток, на прохождении комната уровня безумный. Всего таких комнат на платформе всего 7, на прохождении как раз одна из таких. Сразу скажу, что прохождение заняло не один час. Было много перезапусков комнаты, т.к. машина часто отваливалась. В ходе работы было много подставных данных, которые нам ничего не дают, поэтому если показывать каждый из них, обзор на комнату вышел бы раза в 4 больше. Поэтому итоговый вариант решил оставить исключительно ключевые точки для прохождения. Комната вышла интересной, приятного чтения и изучения!

## Разведка

<img width="1214" height="538" alt="Screenshot from 2026-09-02 12-48-00" src="https//github.com/user-attachments/assets/0939038b-6c2e-4064-bf0d-2849bd7f1170" />

Получив Lab machine, мы начинаем с самой базовой команды для проверки, с чем мы вообще работаем.

<img width="1772" height="1125" alt="Screenshot from 2026-09-02 12-21-10" src="https//github.com/user-attachments/assets/371329af-af6f-46cc-88b3-ac73646c24db" />

Сходу приятно удивило наличие большого количества рабочих портов. Сразу можно понять 2 вещи. Целевая машина на винде и вариантов атаки много (тут сразу была мысль, что веб вообще не факт, что основа атаки). Как всегда начнём с веба, чтобы понять, с чем мы вообще взаимодействуем.

## Веб

<img width="1240" height="799" alt="Screenshot from 2026-09-02 12-21-31" src="https//github.com/user-attachments/assets/59189785-3bca-4de9-8aba-861edbc175c8" />

По картинке и описанию комнаты можно понять, что была проведена атака на сервис злоумышленниками. В итоге всё снесли и осталось это. Ищем скрытые пути.

<img width="1592" height="489" alt="Screenshot from 2026-09-02 12-21-18" src="https//github.com/user-attachments/assets/d3db88ac-9e95-4b28-8242-9e3270a43715" />

Изучаем.

<img width="781" height="225" alt="Screenshot from 2026-09-02 12-21-42" src="https//github.com/user-attachments/assets/c9274655-4612-4426-90cc-1bb5443ec6dd" />

Теперь видим ещё одну точку для проверки и идём поэтапно.

<img width="809" height="296" alt="Screenshot from 2026-09-02 12-21-49" src="https//github.com/user-attachments/assets/3ff06822-7b70-4a06-b5d2-061e0e45ef6b" />

У нас есть первый логин, пароль и внутреннее серверное имя. Забегая наперёд это абсолютно нигде не пригодится. Скорее всего это был вектор атаки злоумышленников.

<img width="1233" height="294" alt="Screenshot from 2026-09-02 12-22-47" src="https//github.com/user-attachments/assets/294a89d9-dae0-4857-94ac-f3d78d2c8006" />

Тут изначально думаешь об открытом RCE, но увы, это простенький скрипт, который просто работает с "hello". То есть очередная не нужная информация. Спустя большое количество иных проверок было установлено, что веб не является точкой для атаки, НО

<img width="1148" height="257" alt="Screenshot from 2026-09-02 12-22-55" src="https//github.com/user-attachments/assets/2b65468c-92cd-4011-a906-6e1c3672f5dd" />

Тут были юзеры атакующих, это нам и пригодилось далее.

## Энумерация домена

Создаём список этих юзеров в 2 форматах и прогоняем в "kerbrute", и находим 3 юзеров. Вероятно это были юзеры для закрепления. Когда я прогонял список юзеров из словаря в 14к, то находил немало юзеров, но все они не имели никакого вектора развития, включая брутфорс, но с этими 3 другая ситуация.

<img width="2034" height="432" alt="Screenshot from 2026-09-02 12-24-45" src="https//github.com/user-attachments/assets/f2cee4bf-43d6-461e-ab9d-6719981e4070" />

Лёгкие проверки не увенчались успехом, пробуем брут.

<img width="1076" height="432" alt="Screenshot from 2026-09-02 12-26-09" src="https//github.com/user-attachments/assets/fcaa33e8-1652-4d6e-9c47-da0803809a0b" />

<img width="1666" height="184" alt="Screenshot from 2026-09-02 12-27-16" src="https//github.com/user-attachments/assets/3c71af87-e3b9-4ade-bbff-18498257dfba" />

<img width="1666" height="125" alt="Screenshot from 2026-09-02 12-27-46" src="https//github.com/user-attachments/assets/11b973eb-ad20-47ef-8805-701d97f75ef4" />

И мы находим первую учётку юзера. Нужно посмотреть, что на неё вообще есть, и искать вектор выше.

## Перечисление домена

<img width="1664" height="577" alt="Screenshot from 2026-09-02 12-31-39" src="https//github.com/user-attachments/assets/c0d1905e-a40c-4300-b6c2-7a7afa794a67" />

<img width="2129" height="237" alt="Screenshot from 2026-09-02 12-32-26" src="https//github.com/user-attachments/assets/7a5eda95-b13b-49cd-8aad-6ee5656b0882" />

<img width="1840" height="818" alt="Screenshot from 2026-09-02 12-32-55" src="https//github.com/user-attachments/assets/2b93e5c4-5706-4ae4-85a6-88f95660e9cd" />

Мы находим очень странного юзера с некорректными правами. Для начала давайте его получим.

## Кербероастинг

<img width="2138" height="296" alt="Screenshot from 2026-09-02 12-33-46" src="https//github.com/user-attachments/assets/7dd11da5-9186-4354-bc40-c7bf9cecdf0f" />

<img width="1207" height="49" alt="Screenshot from 2026-09-02 12-34-09" src="https//github.com/user-attachments/assets/6b89c03e-d393-4356-bf11-5f17d4b69381" />

<img width="2138" height="424" alt="Screenshot from 2026-09-02 12-34-21" src="https//github.com/user-attachments/assets/1d3a0526-ca56-4396-94e9-a68a6e41e8f2" />

Сбрутили, и пароль весьма простой вышел, но что же с этим юзером не так?

<img width="1665" height="205" alt="Screenshot from 2026-09-02 12-35-33" src="https//github.com/user-attachments/assets/ed1663d7-6509-49d9-9ac9-da5cfdb52f19" />

## Делегирование билет администратора"
Вышло целых 4 проблемы. Во-первых, пароль, который брутится за секунду. Во-вторых, аккаунт был с SPN, то есть любой юзер домена может запросить его сервисный билет и ломать пароль офлайн, что мы и сделали. В-третьих, делегирование с переходом протокола "trustedtoauth". Обычно ограниченное делегирование пересылает билет реального пользователя, но в данном случае шаг кербероса S4U2self позволяет этому аккаунту буквально сказать "запрос пришёл от Пети", без какого-либо участия самого Пети. Проще говоря, появляется возможность представляться другим юзером, например админом. Само по себе это ещё не админские права, но примут такую подделку, определяет белый список делегирования. И тут четвёртая проблема "msDS-AllowedToDelegateTo" - это белый список служб, к которым аккаунту разрешено делегировать чужие личности, и там записан "oakley/DC.COOCTUS.CORP" - то есть целевая служба на контроллере домена, к которой разрешено делегирование. Казалось бы, нам нужен cifs, а в списке только oakley - как мы тогда получим админа? Дело в том, что при S4U2proxy KDC сверяет запрос с белым списком по хостовой части SPN (всё, что после "/"), а не по типу службы. Все службы одной машины, включая нужный нам cifs, технически принадлежат одному компьютерному аккаунту DC$ и шифруются одним машинным ключом. Для KDC вопрос звучит не "разрешён ли доступ к службе cifs?", а "разрешено ли делегирование на машину DC.COOCTUS.CORP?". Разрешено - запись "oakley/DC.COOCTUS.CORP" указывает ровно на эту машину. Значит, пройдёт запрос на любой другой тип службы того же хоста "cifs", "ldap", "rpcss" - абсолютно не важно. Поэтому флаг "-altservice cifs/DC.COOCTUS.CORP" в "getST" формально просит у KDC билет, у которого в поле имени службы стоит "cifs/DC.COOCTUS.CORP" проверка по хосту проходит, билет выдаётся настоящий, зашифрованный машинным ключом контроллера, и SMB-сервер его принимает. То есть ряд таких ошибок дал нам возможность сказать "я админ" и при этом получить билет, который реально даёт эти права. Дальше естественно остаётся просто забрать все флаги.
## Флаги

Хэши есть, и мы легко получаем "золотой билет". По сути у нас полное владение хостом. Идём за флагами.

<img width="1190" height="549" alt="Screenshot from 2026-09-02 12-36-18" src="https//github.com/user-attachments/assets/f1036952-8172-467e-b475-8a7c9f918344" />

В очередной раз видим, что мы админ.

<img width="968" height="146" alt="Screenshot from 2026-09-02 12-36-43" src="https//github.com/user-attachments/assets/6b5f9376-0e1f-42d2-80e7-d75dda02bd08" />

<img width="1335" height="335" alt="Screenshot from 2026-09-02 12-40-44" src="https//github.com/user-attachments/assets/de2bde85-a431-4617-819f-b8b2cb547e8d" />

Тут у нас лежат 3 флага. Спокойно всё забираем.

<a id="flag1" class="anchor"></a>
<img width="1337" height="166" alt="Screenshot from 2026-09-02 12-37-02" src="https//github.com/user-attachments/assets/2ea115cd-33da-407b-8e74-e7679f4d9dd2" />

Первый флаг забрали.

<a id="flag2" class="anchor"></a>
<img width="1337" height="166" alt="Screenshot from 2026-09-02 12-37-21" src="https//github.com/user-attachments/assets/cba4ee19-a40a-4718-88c6-13c4bc50122c" />

Второй флаг забрали.

<a id="flag3" class="anchor"></a>
<img width="1337" height="166" alt="Screenshot from 2026-09-02 12-37-52" src="https//github.com/user-attachments/assets/d65827de-7e77-4e68-82b0-926ca2bed553" />

Третий флаг тоже забираем и ищем рут файл.

<img width="1337" height="166" alt="Screenshot from 2026-09-02 12-39-44" src="https//github.com/user-attachments/assets/e37478c7-0486-436f-8642-e89d4b2e6088" />

Идём забирать файл.

<a id="flag4" class="anchor"></a>
<img width="1337" height="166" alt="Screenshot from 2026-09-02 12-40-04" src="https//github.com/user-attachments/assets/1324a8e0-088c-4d71-97ed-fc5b74d81c2c" />

Рут флаг получен.

## Мнение о комнате

Комната вышла весьма насыщенной и технически сложной. Нет привычного у CTF комнат, которые дают явные подсказки, явный путь развития. Тут почти любой тип развития выглядит как в теории возможный, но на практике тупик. Фейковые данные и куча не нужного мусора. Очень хорошая комната для изучения. На самом деле я даже пробовал пробивать через CVE, но было многое зашито, и успеха не возымело. Классная комната, которая поднимает важную тему билетов.
