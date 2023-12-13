
<h1>Экзамен по предмету "Компьютерные сети"</h1>    
<li>Сдающий: <b>Ващук Лев Александрович  </b>  
<li>Группа: <b>з5130902/10202</b>  
<li>Вопрос: <b>Вопрос 9 </b> 

[Задачи транспортного уровня](#транспортный-уровень-задачи-транспортного-уровня) <br>
[Транспортный уровень](#транспортный-уровень-задачи-транспортного-уровня) <br>
[Протокол UDP](#протокол-udp) <br>
[Протокол TCP](#протокол-tcp-формат-заголовка) <br>
[Сокеты Беркли](#сокеты-беркли-и-их-интерфейс) <br>
</li>
  

## Транспортный уровень, задачи транспортного уровня
Чтобы раскрыть тему транспортного уровня в модели OSI надо углубиться в саму OSI. <br>  

  
Сетевая модель OSI (The Open Systems Interconnection model) — сетевая модель стека сетевых протоколов  OSI/ISO. _Посредством данной модели различные сетевые устройства могут взаимодействовать друг с другом._ <br>

_**Самым важным для нас моментом является определение различных уровней взаимодействия систем. Каждый уровень выполняет присущие ему функции.**_
Модель становится намного понятней, когда мы можем ее визуализировать, как на рисунке 1.


<img src="https://skillbox.ru/upload/setka_images/12504801092022_ac4aa360577ec93ee375b17d290f20d8751c3719.png">

<p align=center>Рисунок 1 - наглядный пример модели OSI</p>

Думаю будет очень полезно описать в кратце каждый уровень для более четкой общей картины
<li>Прикладной уровень

_Прикладной уровень отвечает за определение конкретного типа самого приложения. Например, браузер, контейнер сервлетов, веб сервер, база данных._</li>
<li>Уровень представления

_Этот уровень отвечает за преобразование протоколов и кодирование/декодирование данных. Запросы приложений, полученные с уровня приложений, он преобразует в формат для передачи по сети, а полученные из сети данные преобразует в формат, понятный приложениям._ 

_**Из реальной жизни можно привести пример строковый тип данных, в которых лежат  SQL запросы, в которые через знак `?` в синтаксисе языка передают параметры. Прошу обратить внимание оператор `WHERE`, в ниже приведенном запросе**_
```
/---VisualFoxPro 9---/
PRIVATE pcExampleVar1, pnExampleVar2, pnExampleVar3

pcExampleVar1 = alltrim(thisform.txtField1.value)
pnExampleVar2 = thisform.txtField2.value
pnExampleVar3 = thisform.txtField3.value

PRIVATE lcStatement
lcStatement = ";
SELECT DISTINCT a.field1;
FROM a mytable;
WHERE a.otherField1=?pcExampleVar1 AND;
a.otherField2=?pnExampleVar2 AND;
a.otherField3=?pnExampleVar3"
```
Такой запрос будет обработан вашим драйвером СУБД. В моем случае ODBC (The Microsoft Open Database Connectivity (ODBC) обработает этот запрос уже после компиляции программы и подставит параметры из указаных переменных.
</li>
<li>Сеансовый уровень

Сеансовый уровень предназначен для установления, поддержания и завершения соединения между приложениями участников соединения. В качестве примера можно привести GSM. Сеансовый уровень используется между мобильным телефоном MS и коммутатором мобильной сети MSC для установления исходящего либо входящего соединения</li>
<li name="transport">Транспортный уровень

Четвертый уровень сетевой модели OSI, **предназначен для доставки данных**.  Работает в обоих направлениях. Способен принимать и отсылать информацию. Предоставляет сам механизм передачи.

Транспортный уровень модели OSI (равно как и стека протоколов [TCP](#протокол-tcp-формат-заголовка)/IP) обеспечивает следующие возможности:
1. Отслеживание индивидуальных сеансов общения между приложениями на передающем и принимающем устройствах;
2. Сегментация данных (разбиение больших порций данных на сегменты для индивидуальной отсылки по сети, сборка этих сегментов после получения);
3. Идентификация приложений, передающих и принимающих данные.

Траспортный уровень имеет 2 режима работы: [TCP](#протокол-tcp-формат-заголовка) и [UDP](#протокол-udp), которые будут рассмотрены ниже, **но ради полноты пункта можно сказать, что TCP более надеждый режим, но не такой быстрый как датаграммы UDP, которые, например, используются в стриминговом сервисе Twitch при проведении трансляций видео.** 



</li>
<li>Сетевой уровень

Этот уровень прежде всего должен прокладывать маршрут между узлами. **Тут мы хотим передавать данные по большой сети, то есть не факт, что напрямую, а значит придется осуществлять routing.** 
В больших сетях топология постоянно изменяется, поэтому необходимо изменять стратегии доставки сообщений в зависимости от этих изменений, а также в зависимости от загруженности сети. Для решения этой задачи существуют алгоритмы маршрутизации, которые решают куда нужно отправить сообщение.
</li>

<li>Канальный уровень

Второй уровень сетевой модели OSI  предназначенный для передачи данных узлам, находящимся в том же сегменте локальной сети.
</li>
<li>Физический уровень

Нижний уровень модели OSI — физическая и электрическая среда для передачи данных. 
***Хочу отметить, что этот уровень интересен тем, что именно на него больше всего может повлиять обычный пользователь. Речь идет про сетевой адаптер, который несет на себе шифратор и дешифратор  информации. Просто докупив более продвинутую сетевую карту можно, например, повлиять на ping в online видеоиграх.***
</li>




## Протокол UDP
User Datagram Protocol — протокол пользовательских датаграмм. Один из немногих ключевых элементов набора сетевых протоколов для Интернета. Считаю, что надо сразу противопоставить этот протокол протоколу [TCP](#протокол-tcp-формат-заголовка) и определить разницу между ними

 - *С UDP компьютерные приложения могут посылать сообщения (датаграммы) другим хостам **без необходимости предварительного сообщения для установки специальных каналов** передачи данных.*
 - *C TCP предоставляет поток данных с **предварительной установкой соединения, осуществляет повторный запрос данных в случае потери данных и устраняет дублирование при получении двух копий одного пакета**, гарантируя тем самым целостность передаваемых данных и уведомляет отправителя о результатах передачи.*
 
 Теперь, когда линия между протоколами проведена давайте посмотрим во внутрь самого главного UDP - его датаграмму на рисунке 2.

<p align=center>
<img  src="https://www.luckycom.ru/wp-content/uploads/2018/06/header-UDP.gif">
</p>

<p align=center>Рисунок 2 - внутренности датаграммы</p>

Заголовок UDP всегда имеет длину 64 бита. Поля, определённые в сегменте UDP включают следующие: 

 1. *Порт отправителя (Source port): номер порта источника(16 бит);*
 2. *Порт получателя (Destination port): номер порта назначения (16 бит);*
 3. *Длина сообщения (Length): длина заголовка UDP и данных UDP (16 бит);*
 4. *Контрольная сумма (Checksum): вычисленная контрольная сумма полей заголовка и данных (16 бит)*;
(Любопытно, что при всей сути датаграммы в ее заголовке все равно есть чексумма. UDP готов потерять пакет, но следит за его целостностью.)
 5. *Данные (Data): данные протокола вышележащего уровня (upper-layer protocol – ULP) (переменная длина).*
 
 Заголовок несет в себе 64 бита информации и используется в UDP: TFTP, SNMP (почта), Network File System (NFS, удаленные директории) и Domain Name System (DNS).
 


## Протокол TCP, формат заголовка

Долгое время я не понимал до конца что такое TCP и стэки протоколов, которые с ним связаны. Постоянно шла речь про какие-то "пакеты". **В видеоиграх часто можно услышать про "патерю пакетов"**, а что это за пакеты, что они в себе содержат я не понимал. Уже позже начальник на работе мне поверхностно прояснил ситуацию, но теперь стоит посмотреть еще грубже.
Думаю, что хорошим примером будет связка HTTP и TCP/IP. 
TCP - протокол управления передачей <ins>данных</ins>, а HTTP в этом случае являются <ins>данными</ins>.

***Протокол передачи данных HTTP просит TCP установить соединение для передачи запросов. TCP  в этой ситуации является мостом между отправителем и получателем (опираясь на IP адресацию). Запрос HTTP оборачивается в пакеты в соотвествии с TCP заголовком, который продемонстрирован на рисунке 3 и следует по указанному адресу.***

<p align=center>
<img  src="https://github.com/narcissusTheFlower/networking-exam/blob/master/tcp_header.png">
</p>

<p align=center>Рисунок 3 - внутренности заголовка TCP</p>

Заголовок  TCP несет в себе следующие составляющие:
1. Порт отправителя (Source port): номер порта источника (16 бит);
2. Порт получателя (Destination port): номер порта назначения (16 бит);
3. Порядковый номер первого октета данных сегмента, используемый для гарантии правильного упорядочения приходящих данных (32 бита);
4. Номер подтверждения (Acknowledgment number): следующий ожидаемый октет TCP (32 бита);
5. Длина заголовка (Header length): количество 32-битных слов в заголовке (4 бита);
6. Зарезервировано (Reserved): установлено в 0 (3 бита);
7. Управляющие биты (Control bits): функции управления – такие как установка, перегрузка и разрыв сеанса (9 бит). Одиночный бит, который часто рассматриваемое как флаг;
8. Окно (Window): число октетов (1 октет = 1 байт), которое устройство согласно принять (16 бит);
9. Контрольная сумма (Checksum): вычисленная контрольная сумма полей заголовка и  данных (16 бит);
10. Указатель срочности данных (Urgent): показывает конец срочных данных (16 бит);
11. Опции (Options): в настоящее время определена одна опция – максимальный размер сегмента TCP (0 или 32 бита);
12. Данные (Data): данные протокола вышележащего уровня (upper-layer protocol – ULP) (переменная длина); 

Как мы видим, заголовок TCP тяжелее, чем у UDP. Плата за надежность передачи информации.

 TCP предоставляет поток данных с предварительной установкой соединения, осуществляет повторный запрос данных в случае потери данных и устраняет дублирование при получении двух копий одного пакета, **гарантируя тем самым (отсылка про отличие от UDP) целостность передаваемых данных** и уведомление отправителя о результатах передачи.
 

## Сокеты Беркли и их интерфейс
Выражаясь самым простым языком можно сказать, что "Сокеты Беркли" - это просто библиотека для языка С с поддержкой IPC для программирования сетевых сокетов. 

Когда говорят про данные сокеты часто имею ввиду именно их интерфейс, реализация которого заложена в фундамент стека TCP/IP, благодаря чему считается одной из фундаментальных технологий, на которых основывается Интернет.

Технология сокетов впервые была разработана в Калифорнийском университете Беркли на UNIX-системах. Все современные операционные системы имеют ту или иную реализацию интерфейса сокетов Беркли, так как это стало стандартным интерфейсом для подключения к всемирной сети.

Операции сокетов Беркли делятся на несколько типов:

 1. Первый тип это создание сокетов: Socket, Bind, Listen;
 2. Второй, установка соединения: Connect и Accept;
 3. Третья, передача данных Send, Receive;
 4. Четвёртое, закрытие соединения Close;

**Сокеты Беркли могут работать в одном из двух режимов**: блокирующем или неблокирующем. _Блокирующий_ сокет не возвращает контроль, пока не отошлёт (или пока не получит) все данные, указанные для операции. Это верно лишь для Linux-систем. В других системах, например во FreeBSD, вполне естественно для блокирующего сокета посылать не все данные.

### Работа сокетов
У нас есть два компьютера, клиент и сервер. Вначале необходимо создать сокет на сервере и сделать так, чтобы он мог принимать запрос на соединение.


<p align=center>
<img  src="https://zvondozvon.ru/wp-content/uploads/2019/08/socket.jpg">
</p>

<p align=center>Рисунок 4 - внутренности заголовка TCP</p>

На сервере выполняется вызов Soket. Создается объект — сокет, в простейшем случае, это просто файл (в случае Linux) специального вида.
<hr>
<p align=center>
<img  src="https://zvondozvon.ru/wp-content/uploads/2019/08/bind.jpg">
</p>

<p align=center>Рисунок 5 - внутренности заголовка TCP</p>
Затем вызывается метод Bind, который используется для присоединения сокета к определенному ip адресу и порту. Например, ip адрес из внутренней сети и порт 80, порт веб серверов.
<hr>
Затем сервер вызывает метод сокета accept, это говорит о том, что сервер готов принимать соединения и он переходит в режим пассивного ожидания, ждет установку запросов на соединение от клиентов.
<hr>
<p align=center>
<img  src="https://zvondozvon.ru/wp-content/uploads/2019/08/listen.jpg">
</p>

<p align=center>Рисунок 6 - внутренности заголовка TCP</p>
Вызов Listen говорит о том, что сокет готов принимать соединение по сети, сокет слушает. При вызове listen создаётся очередь для соединений, в вызове необходимо указать размер этой очереди. В примере на картинке ниже, размер очереди 5. Если сервер получит больше, чем 5 запросов на соединение, а предыдущие запросы еще не обработаны, то все новые запросы будут отбрасываться.
<hr>
<p align=center>
<img  src="https://zvondozvon.ru/wp-content/uploads/2019/08/aacept.jpg">
</p>

<p align=center>Рисунок 7 - внутренности заголовка TCP</p>
Клиент со свой стороны, сначала вызывает метод сокет, для создания сокета, как правило для клиента не имеет значение, какой ip адрес и какой порт используется, номер порта назначается операционной системой. Поэтому метод bind на клиентском сокете обычно не вызывается.
<hr>
<p align=center>
<img  src="https://zvondozvon.ru/wp-content/uploads/2019/08/connection.jpg">
</p>

<p align=center>Рисунок 8 - внутренности заголовка TCP</p>
Сразу после создания сокета, вызывается метод connect, в котором указывается ip адрес и порт. В параметрах метода connect указываются ip адрес сервера и порт с которыми нужно установить соединение. Отправляется запрос на соединение.
<hr>

<p align=center>
<img  src="https://zvondozvon.ru/wp-content/uploads/2019/08/copy-socket.jpg">
</p>

<p align=center>Рисунок 9 - внутренности заголовка TCP</p>
Для того, чтобы другие клиенты могли соединяться с этим сервером на этом ip адресе и на этом же порту, создаётся копия сокета. И соединение устанавливается не с исходным сокетом, который принимает соединения, а с копией сокета. Данные передаются через копию сокета.
<hr>

<p align=center>
<img  src="https://zvondozvon.ru/wp-content/uploads/2019/08/receive.jpg">
</p>

<p align=center>Рисунок 10 - внутренности заголовка TCP</p>
Клиент подготавливает порцию данных, вызывает метод send. Данные передаются по сети и сервер может их прочитать с помощью метода receive.
<hr>

<p align=center>
<img  src="https://zvondozvon.ru/wp-content/uploads/2019/08/close.jpg">
</p>

<p align=center>Рисунок 11 - внутренности заголовка TCP</p>
Дальше сервер и клиент могут обмениваться между собой несколькими порциями данных. После того, как все данные переданы, клиент вызывает метод close. После чего происходит разрыв соединения.

