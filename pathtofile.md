path\to\file
============

  
2013-02-04, 22:46  
 Сначала я думал что это баги, но это фичи. Итак:   
   
 1. В Windows \ и / в пути к файлу/папке равноправны. То есть c:\windows\system32\cmd.exe это абсолютно точно то же самое, что и c:/windows/system32/cmd.exe . Забавно, не правда ли? И выглядит необычно.   
 2. Число \ и / между названиями папок не имеет значения, как и их чередование. То есть, совершенно законно можно написать c:\/\/\/\/windows\\system32\\/\//cmd.exe   
   
 Первое, видимо, связано с частичной совместимостью с юником (посикс, все дела), а второе -- как мне подсказали -- с тем, что каждый слэш/бэкслэш считается входом в папку ".", которая ссылается на ту же папку и есть в каждой папке (да-да, в тотал коммандере отображается только "..", "на уровень выше", а ".", "эта же папка" не отображается). То есть folder1\\folder2 раскрывается как folder1\.\folder2, что, разумеется, то же самое, что и folder1\folder2.   
  
<https://diary.ru/~zHz00/p185137051_path-to-file.htm>  
  
Теги:  
[[Программы]]  
[[Наблюдения]]  
ID: p185137051  


Комментарии: 4
--------------

  


---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (1/4) | 2013-02-05, 00:19 | himself | c626206267 |

  
  *В Windows \ и / в пути к файлу/папке равноправны*    
 Не совсем. Правильный разделитель в Windows - \. Ядро и некоторые функции поддерживают и обратный, но исключительно в факультативном режиме. При этом многие стандартные функции во многих языках обратный разделитель не поддерживают, а значит, написанные с их помощью программы тоже. В том числе, возможно, и менее центральные компоненты Windows.   
   
  *Число \ и / между названиями папок не имеет значения, как и их чередование*    
 Опять же,  *только если речь про ядро*  . Никакие пользовательские компоненты таких гарантий не дают (ядро тоже не даёт, но раз однажды так было сделано, теперь так будет всегда... обратная совместимость).   
   
  *с тем, что каждый слэш/бэкслэш считается входом в папку "."*    
 Нет.   
 Даже не знаю, что тут добавить в качестве объяснения. Просто - нет, не считается ![:)](http://static.diary.ru/picture/3.gif)   
 ^c626206267

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (2/4) | 2013-02-05, 00:23 | himself | c626206696 |

  
  [Вот правила составления путей в MSDN](http://msdn.microsoft.com/en-us/library/windows/desktop/aa365247(v=vs.85).aspx)  .   
   
  *Use a backslash (\) to separate the components of a path*    
   
  *Note File I/O functions in the Windows API convert "/" to "\" as part of converting the name to an NT-style name, except when using the "\\?\" prefix as detailed in the following sections.*    
   
 То есть, даже если программа построена чисто на винапи, она всё равно может поперхнуться на пути вида c:/path/, если ради увеличения макс. длины пути передаёт их с префиксом (с префиксом многие функции позволяют до 32767 символов, без - только 260)   
 А уж про чужие библиотеки, тем более, про системное соглашение - вообще ни слова.   
 ^c626206696

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (3/4) | 2013-02-05, 00:46 | zHz00 | c626209063 |

  
 Так. С этим понятно.   
   
 Сначала про \ и /.   
 Где более подробно написано про \\?\, о котором я ничего не знаю?   
 То, что я писал касательно \ и /, я писал касательно портируемости программ вин/линукс друг на друга. Это не было очевидно по посту, поэтому я поясняю это тут. Очевидно, что линуксовые программы не будут использовать расширение \\?\, поэтому эта проблема для них не стоит. Портированные с линукса программы  *действительно*  используют прямой слэш и это работает -- я видел это на примере мплеера и сканирования папки со шрифтами при первом запуске. С путями длиннее 256/260 символов бывает тяжело и не все программы их поддерживают, даже у проводника в хрюше возникают проблемы, хотя он может писать \\?\ и обрабатывать корректно. Слежу, чтобы не было файлов с такими длинными путями.   
   
 Про несколько \ хотелось бы услышать подробнее -- в документации про это и правда ничего нет, однако как следует относиться к, допустим, переходу c:\\windows? Теоретически это два перехода в подпапку, при этом первая подпапка пустая. Чем же она формально считается?   
 ^c626209063

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (4/4) | 2013-02-05, 01:05 | himself | c626210769 |

  
  [zHz00](https://zHz00.diary.ru "Untitled")  , да в той же статье и написано. Это просто префикс, который говорит "отключи нормализацию и передай строку в ядро как есть". В ядре все пути абсолютные и выглядят как "\\[путь]". Когда функции винапи (пользовательского режима) делают вызов к натив-апи (апи ядра), они приводят пути к ядерной форме. Заменяют слеши на правильные, удаляют дублирующиеся слеши, прибавляют слева текущую директорию, если путь относительный, или "\\.\UNC", если это путь к шаре. А также прибавляют "\\.\". А если строка уже начинается с этого, они ничего не делают.   
   
  *Теоретически это два перехода в подпапку, при этом первая подпапка пустая. Чем же она формально считается?*    
 Ничем. Они просто схлопываются в один слеш во время нормализации. Но на самом деле, в отличие от обратных слешей, несколько слешей подряд могут поддерживаться и файловой системой. Я у себя проверил на нтфс в командной строке:   
 \\.\C:\Windows\System32\cmd.exe -- работает   
 \\.\C:\Windows/System32\cmd.exe -- не работает   
 \\.\C:\Windows\\\\System32\cmd.exe -- работает   
   
 Когда ядро разбирает этот путь, оно находит устройство (\\.\C ![:)](http://static.diary.ru/picture/3.gif) . Этой штукой заведует драйвер файловой системы. Винда отрезает найденный кусок и передаёт остаток ему. Получается, что драйвер NTFS и сам тоже нормально реагирует на \\\.   
 ^c626210769