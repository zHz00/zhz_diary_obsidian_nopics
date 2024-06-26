Один казус WinSock
==================

  
2020-11-23, 23:59  
 Эта библиотека пытается быть совместимой с т.н. "сокетами Беркли", поэтому половина функций названы по прототипам 80-х годов. А что не влезло -- по современным. Это создаёт путаницу. Тогда ещё не было методологии именования функций, в т.ч. системных, поэтому функции приёма и передачи данных называются просто recv и send. А что в программе могут быть другие функции отправки и приёма другими способами -- никого не волнует.   
   
 recv просто так не вернёт вам управление, пока не получит данные или пока не сработает таймаут. Да, у меня на приём отдельный поток, но всё равно я хочу иногда чем-нибудь ещё заниматься, а не только ждать, пока мне пришлют запрос. Как же проверить, сколько байт пришло?   
   
 А для этого есть специальный флаговый параметр, который должен быть установлен в MSG\_PEEK. Тогда мы сразу получим то, что уже готово.   
   
 Функция возвращает число типа int. Оно обозначает количество принятых байт. Если нам пока ничего не пришло, сколько байт вернётся? Наверное, ноль.   
   
 Но нет. Ноль зарезервирован для случая отключения второй стороны/ошибки связи. А сколько должна вернуть функция, если ничего не пришло, в документации Microsoft не указано!   
   
 Поэтому я поставил эксперимент и обнаружил, что в случае, если данные не пришли, recv возвращает -1. Это оказалось неожиданно.   
   
   **Пожалуйста, ознакомьтесь с комментариями!**     
  
<https://diary.ru/~zHz00/p220172434_odin-kazus-winsock.htm>  
  
Теги:  
[[Программирование]]  
[[Борьба с техникой]]  
ID: p220172434  


Комментарии: 9
--------------

  


---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (1/9) | 2020-11-24, 15:00 | himself | c749940419 |

  
 Всё там прекрасно указано!   
 
```
If no error occurs, recv returns the number of bytes received and the buffer pointed to by the buf parameter will contain this data received. If the connection has been gracefully closed, the return value is zero.  
Otherwise, a value of SOCKET_ERROR is returned, and a specific error code can be retrieved by calling WSAGetLastError.
```
   
 SOCKET\_ERROR это -1. Так что ты любую ошибку воспримешь как "нет новых данных". Различить можно по WSAGetLastError() и скорее всего, это будет WSAEWOULDBLOCK.   
   
 А вообще,   
 > Как же проверить, сколько байт пришло? А для этого есть специальный флаговый параметр   
 Для этого есть ioctlsocket(FIONREAD), но и он, и PEEK, и select() в лучшем случае гарантируют тебе "что-то пришло", а не точный объём данных.   
   
 Там всё устроено примерно так. У TCP есть внутри небольшой буфер, куда он складывает прилетающие байты. И он сообщает отправителю,  [сколько он ещё готов принимать](https://en.wikipedia.org/wiki/TCP_window_scale_option)  . Когда буфер заполняется, он говорит стопэ, и отправитель подвисает в своём блокирующем send().   
 Вот если у тебя сообщение больше этого буфера, ты его одним махом никогда не получишь.   
   
 Только два способа разбирать TCP надёжно:   
 1. Создать буфер нужного размера, передать его в winsock и смиренно ждать, когда его заполнят. (винсок сам будет вынимать байты в твой буфер по мере поступления)   
 2. Немедленно вынимать из recv() всё, что там становится доступным, и класть в свой собственный буфер, который уже разбирать как тебе нравится.   
   
 Второе сделать можно кучей способов: peek + recv (неэффективно: читаешь дважды), FIONREAD+recv (норм), select+FIONREAD+recv (чтобы эффективно спать, пока ничего нет), включить неблокирующие сокеты и пробовать время от времени (плохо), включить неблокирующие сокеты и спать на эвенте, включить неблокирующие сокеты и получать оконные события, включить неблокирующие сокеты и делать overlapped (ты запустил чтение и тебе скажут, когда оно сделалось).   
   
 Из всего этого overlapped это самое эффективное, поскольку ты по сути пульнул заявку в режим ядра и ушёл по своим делам, но оно нужно только при сотнях подключений. А самое простое это висеть в recv намертво, если у тебя отдельный поток, а когда поток надо завершать, то сказать ему снаружи crash and burn ( closesocket() + WSACancelBlockingCall() )   
 ^c749940419

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (2/9) | 2020-11-24, 15:34 | zHz00 | c749940884 |

  
  [himself](http://himself.diary.ru "void")  , ультимативный гайд, спасибо!   
 ^c749940884

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (3/9) | 2020-11-24, 19:00 | Xersareeth | c749943781 |

  
 Мммм, сколько здесь знатоков... =)   
   
 А я был уверен что tcp передаёт непрерывный поток байт и вот такая постановка вопроса в принципе не верна:   
 > Вот если у тебя сообщение больше этого буфера, ты его одним махом никогда не получишь.   
   
 Это же как читать файл или пайп. ХЗ сколько там ещё байт, никто ничего не гарантирует.   
 ^c749943781

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (4/9) | 2020-11-24, 20:12 | himself | c749944987 |

  
  [Xersareeth](http://BurrowDeclassified.diary.ru "One more fang")  , рад, что помог узнать что-то новое ![:)](http://static.diary.ru/picture/3.gif)   
   
  [Вот ещё полезная ссылка](https://tangentsoft.net/wskfaq/articles/lame-list.html)  .   
 ^c749944987

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (5/9) | 2020-11-24, 21:36 | deadlymercury | c749946763 |

  
 Эээ, а как tcp может передавать непрерывный поток байт, если там при этом идет проверка контрольных сумм и подтверждения доставки?)   
 ^c749946763

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (6/9) | 2020-11-25, 19:51 | Xersareeth | c749959447 |

  
  [deadlymercury](http://crazysupp.diary.ru "Записки безумного саппорта")  , вероятно точно так же, как в файлах может храниться непрерывный поток байт, хотя там кластеры, секторы и чёрти-чо ещё, в том числе и проверка контрольных сумм =)   
 ^c749959447

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (7/9) | 2020-11-25, 22:24 | deadlymercury | c749962244 |

  
 Так tcp это же гораздо более низкоуровневая штука, чем "файл"?)   
 ^c749962244

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (8/9) | 2020-11-25, 22:32 | Xersareeth | c749962411 |

  
 Дыа? И на каком же уровне модели osi находятся файлы?)   
 ^c749962411

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (9/9) | 2020-11-26, 00:10 | deadlymercury | c749964104 |

  
 где-то над фс, который при этом тоже менее низкоуровневая штука, чем tcp ![;)](http://static.diary.ru/picture/1136.gif) Хотя при этом tcp более высокоуровневая штука, чем сектора на диске.   
 ^c749964104