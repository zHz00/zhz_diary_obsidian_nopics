const
=====

  
2015-06-15, 23:58  
 Чтобы выяснить, по какой ветви алгоритма идти дальше, программа смотрит старший байт числа, получаемого из dll-ки и проверяет, равен он 1 (...01) или 2 (...10).   
   
 Число несёт две функции -- старший байт зависит только от версии библиотеки, а младший зависит от ряда обстоятельств.   
   
 Раньше всё работало, потому что число было фиксированным. Но потом я сделал, что младшие разряды принимают различные значения (так надо было сделать изначально, фиксированное значение было заглушкой).   
   
 После усовершенствования всё перестало работать, потому что старший байт не оказывался равен ни 1, ни 2. Стал разбираться.   
   
 Сначала оказалось, что я просто забыл его установить. Установил:   
   
 a|=0x0200;   
   
 (хотя значение младших и должно было меняться, старший байт зависел только от версии dll-ки, так что константа тут -- ок).   
   
 Не работает. Сделал вывод числа. Он мне показал, что старший байт равен -1.   
   
 Как?   
   
 Короче говоря, проверка значения a осуществлялась ещё до его инициализации из файла настроек, а значение по умолчанию (в конструкторе) было у него -1.   
   
 a=-1;   
 a|=0x200;//a==-1   
 Почему? Отрицательные числа представляются в дополнительном коде. Не вдаваясь в подробности, скажу, что минус единица выглядит как 0xFFFF. Поэтому поразрядное "или" с чем угодно даст ту же минус единицу.   
   
 Осталось понять, почему проверка тоже даёт минус единицу.   
   
 Чтобы проверить старший байт, в программе число сдвигается на 8 бит вправо. Если сдвиг арифметический, то должно остаться -1, что и происходит.   
 a>>=8; //a==-1   
   
 Почему должна остаться -1? Тут надо сказать, что сдвиг бывает арифметический и логический. Логический просто сдвигает разряды как есть. В случае логического получилось бы 0xFFFF>>8==0x00FF (каждая шестнадцатеричная цифра -- 4 бита). Арифметический же не трогает старший, знаковый бит (у отрицательных чисел он равен единице). И при сдвиге дублирует его слева, т.е. при арифметическом сдвиге четырёхразрядного двоичного числа 1011 получается 1101. Старший бит не трогаем, второй дублирует старший, первый это бывший второй, нулевой это бывший первый, ну а бывший нулевой пропал.   
 Тут становится понятно, что арифметический сдвиг вправо числа 0xFFFF на  *любое*  число разрядов так и оставит это число неизменным.   
   
 Поскольку проверка старшего байта и должна была производиться до инициализации с использованием файла настроек, я решил эту проблему, установив в конструкторе значение 255 (0x00FF) для числа, что вернуло нормальное поведение поразрядного "или" и сдвига.   
  
<https://diary.ru/~zHz00/p204604138_const.htm>  
  
Теги:  
[[Программирование]]  
ID: p204604138  


Комментарии: 8
--------------

  


---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (1/8) | 2015-06-16, 04:28 | Гость | c690565666 |

  
 а ты не оправдывайся!   
 ^c690565666

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (2/8) | 2015-06-16, 13:03 | korrshun | c690578714 |

  
 пгостите, а какого лешего используется для флагов signed-переменная?   
 ^c690578714

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (3/8) | 2015-06-16, 13:46 | zHz00 | c690580964 |

  
  [korrshun](http://Igel-kun.diary.ru "kimi wo shiranai monogatari")  , исторически. Но ты прав -- надо было делать унсигнед. Тогда бы и -1 не прокатило.   
 ^c690580964

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (4/8) | 2015-06-16, 14:55 | zHz00 | c690584651 |

  
  **Гость**  ,   
 >>а ты не оправдывайся!   
   
 а ты няша!   
 ^c690584651

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (5/8) | 2015-06-16, 16:13 | korrshun | c690588625 |

  
 ну -1 использовать для инициализации флаговых переменных - это тоже в районе зла.   
 ^c690588625

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (6/8) | 2015-06-16, 17:11 | zHz00 | c690591500 |

  
  [korrshun](http://Igel-kun.diary.ru "kimi wo shiranai monogatari")  , признаю.Только в момент, когда писали -1, про флаги речи не шло))).   
 ^c690591500

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (7/8) | 2015-06-16, 19:19 | Гость | c690597665 |

  
  а ты няша!    
   
 а ты не оправдывайся!   
 ^c690597665

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (8/8) | 2015-06-16, 21:41 | zHz00 | c690606426 |

  
 >>Только в момент, когда писали -1, про флаги речи не шло))).   
   
 Точнее, шло, но я об этом ещё не знал!))))))))   
 ^c690606426