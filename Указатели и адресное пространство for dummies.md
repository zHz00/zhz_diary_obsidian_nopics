Указатели и адресное пространство for dummies
==============================================

   
 2017-11-29, 23:25   
  Понимание сути указателя не может быть "поверхностным" или "глубоким". Либо оно есть, либо его нет.   
   
  [(читать дальше)](https://zHz00.diary.ru/p214292238.htm?index=1#linkmore214292238m1)    Поэтому я старался всегда начать рассказ об указателях как можно раньше. И повторял рассказ многократно.   
   
 Не уверен, что я достиг успеха в об'яснении этого ключевого понятия студентам. Но уверен, что не достиг успеха в понимании того, поняли они его или нет (и это гораздо хуже).   
   
 Тема работы с указателями актуальна далеко не для всех. Некоторые языки предоставляют только работу со ссылками (порой, замаскированными). Это непрозрачно, поскольку каждая ссылка в глубине души всё равно является указателем. Однако работа со ссылками выглядит как работа с простыми переменными. Я считаю это основным отличием указателя от ссылки.   
   
 В этом смысле Си/Си++ на коне, т.к. там всегда понятно, о чём речь. С указателями вообще всегда, а со ссылками -- придётся посмотреть определение. В Delphi, к примеру (и это в своё время стало для меня откровением) совершенно идентичные по форме определения означают разные вещи. Например: 
```
var  
	t: integer;//переменная  
	o:TButton;//ссылка (указатель) на об'ект типа TButton
```
   
 Иногда в учебниках пишут, что указатель это ссылка (на об'ект). Не то чтобы это было совсем неправильно. Однако после этого требуется понять, что значит слово "ссылка". И что это вообще даёт. Мне кажется, что слово "ссылка" не может дать понимание сути указателя, поскольку является слишком неконкретным. Я уже не говорю о случаях, когда ссылки существуют в языке как самостоятельная сущность. Тогда есть риск скатиться в  [сепульки](https://lurkmore.to/%D0%A1%D0%B5%D0%BF%D1%83%D0%BB%D1%8C%D0%BA%D0%B8)  .   
   
 С моей точки зрения, единственным верным определением является следующее: "указатель -- это  **адрес**  того, на что он указывает" (часто пишут, что указатель это  **переменная, содержащая адрес**  , однако это может быть и константа, вообще-то). После чего необходимо рассказать о том, что такое адресация в памяти. Об этом в любом случае следует рассказывать, т.к. низкоуровневое понимание работы компьютера и ОС улучшает понимание высокоуровневых концепций программирования.   
   
 Хороши об'яснения при помощи аналогий. Я придумал когда-то одну.   
   
 Представьте себе улицу, на которой стоят дома: 1, 2, 3 и т.д. На некоторых домах есть фамилия жильца: Иванов, Петров, Сидоров... Работа с переменной -- это общение с человеком по имени Иванов, к примеру. Заходим к нему в дом -- а там он сидит. Работа с указателем -- это не то. Петров работает связистом. Мы заходим к Петрову, а он говорит -- иди в дом 51. Мы приходим туда -- и там то, что мы ищем. На доме может быть написано "Наталья Гончарова". А может и ничего не быть написано. Если указатель двойной, то в доме 51 будет ещё один связист.   
   
 Номер дома это как раз адрес. А фамилия -- имя переменной. Переход, куда говорят -- это разыменование.   
   
  Естественно, что обращение к любой переменной тоже осуществляется через адрес, но механизм немного другой. Во-первых, мы заранее знаем, где переменная расположена, поэтому нам не надо проверять значение другой переменной (заглядывать к Петрову; Петров-то может каждый раз новое указание выдавать). А во-вторых, обычные локальные переменные адресуются относительно указателя стека, т.е. их адрес имеет вид SP+X, где X -- относительный адрес переменной, а SP -- фиксированный адрес области стека, предназначенной для текущего вызова функции. Если продолжать аналогию с улицей, то нумерация домов на всех улицах сквозная, а когда приходит бригада рабочих и заселяется, они всегда заселяются в одном и том же порядке. Поэтому мы знаем, что Иванов будет всегда жить в первом доме на улице, а Петров -- во втором (какие бы номера у домов не были). 51 же -- это сквозной номер дома, он отсчитывается не от начала улицы, а по всем улицам.    
   
 Уж не знаю, насколько такой метод помогал пониманию.   
   
 Недавно я придумал ещё одну аналогию, более точную и более понятную. Но сейчас я занятия у студентов не веду, так что тестировать не на ком.   
   
 Телефонный номер.   
   
 Если человек понимает, что такое переадресация звонка, то он, фактически, уже понимает, что такое указатель и его разыменование! Ведь телефонный номер это и есть форма адреса в памяти. Кроме того, тут же можно провести аналогию ещё и с адресным пространством, что для улицы было сложновато. Адресное пространство это то же, что и номерная ёмкость. То есть, если номер однозначный, то доступно всего 10 номеров телефонов -- от 0 до 9. Если семизначный -- от 000-00-00 до 999-99-99. И это не значит, что все номера заняты. Первые три цифры в Москве раньше были номером АТС (сейчас не знаю, как). И некоторые АТС вполне могут отсутствовать. Тогда мы никуда не дозвонимся. Можно ещё дозвониться не туда, позвонив, к примеру, по случайному номеру в действующей АТС, но совершенно другой случай.   
   
 Это совпадает с тем, что происходит в реальной памяти: не все диапазоны адресов соответствуют ячейкам. Фактический размер памяти, как правило, меньше адресного пространства.     
    
 <https://diary.ru/~zHz00/p214292238_ukazateli-i-adresnoe-prostranstvo-for-dummies.htm>   
   
 Теги:   
 [[Программирование]]   
 [[Студенты]]   
 [[Статьи]]   
 ID: p214292238