Программирование на Си с помощью Excel
=======================================

   
 2012-03-23, 23:41   
   *Описан забавный приём полу-автоматического создания исходного текста с применением Microsoft Excel.*    
  [(читать дальше)](https://zHz00.diary.ru/p174430378.htm?index=1#linkmore174430378m1)      
 Ну это вообще. Допустим есть что-то вроде:   
 
```
  
struct q<br>	{<br>		int field1;<br>		double field2;<br>		int field3;<br>		int field4;<br>...<br>		int fieldN;  
	};
```
   
   
 А надо сделать что-то вроде:   
 
```
  
printf("%d\n",t.field1);  
printf("%lf\n",t.field2);  
printf("%d\n",t.field3);  
printf("%d\n",t.field4);  
...  
printf("%d\n",t.fieldN);  

```
   
 где t -- структура типа q. Названия полей не такие удачные как тут, а рандомные.   
 Что мы делаем? Мы открываем эксель. Вставляем туда (Правка->Специальная вставка; после этого поводите мышью около правого нижнего угла выделения, там будет "смарт-тег"; откройте его и выберите "мастер импорта текст", там выберите разделитель пробел (или что у вас там)):   
 
```
  
  
		int field1;  
		int field2;  
		int field3;  
		int field4;  
...  
		int fieldN;  

```
   
 Тогда в первой колонке будет тип, а во второй -- имя поля. Проводим автозамену (Ctrl+H) с удалением точек с запятой. Оттаскиваем колонки на одну вправо, в самой левой пишем:   
 
```
printf(".
```
   
 Потом тиражируем (если потянуть за угол клетки, содержимое будет копироваться на всё выделение). Потом автозаменой все int -> %d, double -> %lf. Потом оттаскиваем колонку с названиями полей, между ней и тем что справа вставлеем:   
 
```
\n",t.
```
   
 А в самую правую (пока пустую) колонку пишем:   
 
```
);
```
   
 Получится примерно вот такое:   
  ![](http://s019.radikal.ru/i644/1203/16/194449bab2cf.png)    
 Что дальше? Надо это как-то слить. Выделяем, копируем и вставляем в Блокнот (notepad.exe). Получается:   
 
```
  
printf("	%d	",t.	field1	);  
printf("	%lf	",t.	field2	);  
printf("	%d	",t.	field3	);  
printf("	%d	",t.	field4	);  
printf("	%d	",t.	fieldN	);  

```
   
 Почти. Осталось убрать табуляции. Выделяем мышкой один из символов (он невидимый), копируем, проводим автозамену на пустое место. Вуаля:   
 
```
  
printf("%d",t.field1);  
printf("%lf",t.field2);  
printf("%d",t.field3);  
printf("%d",t.field4);  
printf("%d",t.fieldN);  

```
   
   
 Разумеется, для пяти полей это нецелесообразно. Однако данный метод неоднократно применялся мной для гораздо большего числа полей и сложных конечных конструкциях.   
     
    
 <https://diary.ru/~zHz00/p174430378_programmirovanie-na-si-s-pomowyu-excel.htm>   
   
 Теги:   
 [[Программирование]]   
 [[Статьи]]   
 ID: p174430378