Untitled [352]
==============

  
2015-02-17, 23:59  
 Приложение Яндекс.Транспорт  *знает*  о моём местонахождении на ведроиде. При этом в настройках системы галочки, разрешающие программам определять моё положение по GPS/WiFi сетям СНЯТЫ! Но у приложения есть разрешение на определение положения -- т.к. оно требуется при установке.   
   
 Это как понимать?   
 а) галочка, не разрешающая получать данные, игнорируется, т.к. есть разрешение. Непонятно, в чём смысл галочки тогда, т.к. приложения, у которых разрешения нет, и так не смогут этого делать, а те, у кого есть, галочку игнорируют.   
 б) Яндекс как-то получает инфу о Wi-Fi сетях в обход системы разрешений. Это очень плохо.   
   
 Вот что из этого? В любом случае, моя паранойя агитирует меня не включать WiFi, GPS и GSM (последний и так выключен). Или ставить CyanogenMod.   
  
<https://diary.ru/~zHz00/p202739424_untitled-352.htm>  
  
Теги:  
[[Программы]]  
ID: p202739424  


Комментарии: 13
---------------

  


---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (1/13) | 2015-02-18, 00:16 | deadlymercury | c684023066 |

  
 боян.   
  [habrahabr.ru/post/249747/](http://habrahabr.ru/post/249747/)    
 ^c684023066

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (2/13) | 2015-02-18, 00:50 | korrshun | c684024943 |

  
 Галочки в настройках отключают именно гугловые API навигации по WiFi, и GPS-навигацию.   
 Т.е. если насобирать свою геобазу с местоположениями WiFi-точек, то тебе не надо ходить в гугловую геобазу.   
   
 Я.локатор - это отдельный сервис, которому ты говоришь мак-адрес точки и узнаешь, где ты примерно, кстати доступный всем и каждому.   
 Чтоб узнать мак точки необязательно иметь доступ к сервису определения местоположения, можно просто arp-запрос сделать или еще что-то в таком духе.   
 Такие дела.   
   
 Так ты можешь сделать юзеру хорошо, даже если у тебя нет разрешения делать хорошо. ![:gigi:](http://static.diary.ru/picture/1134.gif)   
   
  [deadlymercury](http://crazysupp.diary.ru "Записки безумного саппорта")  , потом были от яндексоидов еще отмазки на тему того, что это фича, которая стала багом из-за кривого кода.   
 ^c684024943

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (3/13) | 2015-02-18, 01:02 | zHz00 | c684025530 |

  
  [deadlymercury](http://crazysupp.diary.ru "Записки безумного саппорта")  , это немного не то, но имеет прямое отношение. Спасибо.   
  [korrshun](http://Igel-kun.diary.ru "kimi wo shiranai monogatari")  , про гугловские АПИ понял, спасибо.   
   
 Итог: Яндекс действует в обход разрешений т.к. у него свой механизм. Нужно отказаться от сервиса и вырубить все средства связи с внешним миром.   
   
 Интересно, если удалить все приложения от Яндекса, служба суфлирования останется?   
 ^c684025530

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (4/13) | 2015-02-18, 01:13 | korrshun | c684025967 |

  
 Служба суфлирования яндекса снесётся, можешь не волноваться. Остальные останутся.   
 и гугл, и пейсбук, и контактик, и твиттер всё равно продолжат за тобой следить ![:-D](http://static.diary.ru/picture/1133.gif) (вроде, как минимум у пейсбука была своя геобаза и он тоже работает в обход).   
 Можно в /etc/hosts вкатать 127.0.0.1 в качестве ip для хостов локатора и других неугодных, какие найдутся. Еще можно поставить зверя типа 3C Toolbox и всё-всё забанить.   
   
 З.Ы. обо мне и так много знают, так что я забил.   
 ^c684025967

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (5/13) | 2015-02-18, 14:35 | korrshun | c684049300 |

  
 Кстати про CyanogenMod.   
   
  [cyngn.com/legal/privacy-policy/](https://cyngn.com/legal/privacy-policy/)    
 6. Location-Based Services   
   
 Такие дела...   
 ^c684049300

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (6/13) | 2015-02-18, 15:24 | Гость | c684052366 |

  
 Расслабтесь. У властей и так на вас есть полная информация - где вы работаете, чем интересуетесь, чем болели, с кем общаетесь, как выглядите. Они знают все ваши разговоры по телефону, в аське, по почте (все шифровальные сервисы сдают инфу спецслужбам, будьте покойны). Мы все как на ладони.   
 ^c684052366

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (7/13) | 2015-02-18, 16:25 | zHz00 | c684055874 |

  
  [korrshun](http://Igel-kun.diary.ru "kimi wo shiranai monogatari")  , да его всё равно нет для моего проца, как оказалось.   
  **Гость**  , одно дело у властей, а другое дело у каких-то частных организаций.   
 ^c684055874

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (8/13) | 2015-02-18, 23:09 | Гость | c684082901 |

  
  т.к. оно требуется при установке.    
   
 То есть мы пользуемся этим софтом, и ещё возмущаемся. Это условие при установке, иными словами контракт. А заключив контракт и возмущаться по-тому поводу, что вдруг не получается обойти пункт в нём как-то странно. То есть вы при установке приложения надеялись обойти систему, аяяяй, как некрасиво.   
 ^c684082901

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (9/13) | 2015-02-18, 23:47 | zHz00 | c684085675 |

  
  **Гость**  , нет. Контракт -- это лицензионное соглашение. Оно регламентирует, в том числе, то, как будут использоваться полученные данные. Лицензионного соглашения при установке мне показано не было.   
   
 Разрешения же -- это технический список тех функций, которые необходимы программе для работы. Это не одно и то же, т.к. список не содержит Privacy Policy или чего-либо подобного.   
   
 В данном случае речь идёт о другом -- что есть глобальная галочка и локальное разрешение. По идее, они должны делать одну и ту же вещь и одно должно перекрывать другое, но как отмечали комментаторы выше, они делают РАЗНЫЕ вещи.   
   
 >>аяяяй, как некрасиво.   
   
 А следить за пользователями красиво, да?   
 ^c684085675

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (10/13) | 2015-02-19, 01:40 | korrshun | c684091553 |

  
 Лицензионное соглашение по традиции (и гайдам) убирают в раздел "О приложении"   
 Это почти у всех мобильных приложений так. (чтоб чайников канцеляритом не пугать - все равно никто их не читает).   
 Следить в данном конкретном случае красиво, т.к. очевидно, зачем это надо - сделать удобно.   
 Строго говоря, яндекс в данном случае не отслеживает координаты, а наоборот - делает предположение о местонахождении исходя из косвенных факторов.   
 Т.е. передаётся запрос вида "Тут такая wifi-точка, где я вообще?", а не "чуваки, я нахожусь по этим координатам, запишите." Понятно, что первый тип запросов можно трактовать как отслеживание местоположения, но на самом деле это не совсем так. Можно пополоскать в этом месте мозги юристам, пусть решают.   
 ^c684091553

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (11/13) | 2015-02-19, 22:22 | Гость | c684144848 |

  
  А следить за пользователями красиво, да?    
   
 Если не скрывать этого, то вполне нормально. А тут никто не скрывает. Удалите приложение, вы же за него не платили. А если всё-таки хотите пользоваться, то тогда соблюдайте условия указанные при установке.   
 ^c684144848

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (12/13) | 2015-02-19, 23:05 | zHz00 | c684148286 |

  
  [korrshun](http://Igel-kun.diary.ru "kimi wo shiranai monogatari")  , да, в "О программе" есть, но там не указано, что делается, либо не делается с данными о местоположении. Хотя есть ссылка на Privacy Policy.   
   
  **Гость**  , полностью с вами согласен. Удалил.   
 ^c684148286

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (13/13) | 2015-02-23, 15:44 | RetXiRT suiR@ttig@$ | c684367012 |

  
  Ну дык, Тындекс же,  *Кривые Руки*  ™.   
   У властей и так на вас есть полная информация - где вы работаете, чем интересуетесь, чем болели, с кем общаетесь, как выглядите. Они знают все ваши разговоры по телефону, в аське, по почте (все шифровальные сервисы сдают инфу спецслужбам, будьте покойны). Мы все как на ладони.    
 ЛоЛ. Х\*ёво быть вами.    
 ^c684367012