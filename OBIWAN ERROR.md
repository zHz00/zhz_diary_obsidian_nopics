OBIWAN ERROR
=============

   
 2016-05-20, 23:59   
  Рушится куча (heap). Окей. Ставлю Debugging tools for Windows, gflags -p, включаю проверку кучи.   
   
 До этого что-либо диагностировать было крайне сложно, т.к. прога падала всё время в разных местах (причём в системных библиотеках). Теперь она падает в одном и том же. На strcpy.   
   
 Алгоритм такой: сначала из файла читается размер строки, а потом столько байт без проверки считываются во временный буфер, а потом копируются в заданную строку. А вдруг не влезет? Поставил проверки на размер -- прога раньше падает, чем дело доходит до проверок.   
   
 ОКАЗАЛОСЬ   
   
 Выделяется массив строк фиксированной длины. Размер массива равен 100. Потом идёт считывание такого плана:   
 
```
for(x=1;x<=nStrings;x++)  
	{  
		file.read(&size,sizeof(size));  
		file.read(buf,size);  
		strcpy(array[x],buf);  
	}
```
   
   
 АААА!   
   
 Пока количество фактических строк (nStrings) меньше 100, всё работает ок. Но как только размер массива достигает предельного значения -- происходит выход за границы.   
   
 Ошибка стандартная. Её совершил мой начальник лет 10 назад, когда писал этот кусок кода. И обнаружили мы её только из-за того, что один пользователь не стал удалять лишние строки в файле (которые не нужны для работы, но не мешают), а только добавлял новые. Если бы он действовал рационально, мы бы ещё 10 лет ничего не знали.   
    
 <https://diary.ru/~zHz00/p209194806_obiwan-error.htm>   
   
 Теги:   
 [[Программирование]]   
 [[Борьба с техникой]]   
 ID: p209194806