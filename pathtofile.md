path\to\file
=============

   
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