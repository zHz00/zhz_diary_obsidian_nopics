Untitled [492]
==============

  
2017-07-22, 22:32  
 Коллега, которому передали один из проектов, с которым раньше работал я, поставил себе PVS Studio. Статический анализатор кода. И он стал выявлять разные шняги, которые написал, как выяснилось, я (и это подтверждает SVN Blame).   
   
 Вот один из таких кусков, высранных мной в 2013 году.   
 
```
  
    int nx, ny, nresx, nresy;
    char file_list[80][261];
    unsigned stx[80], sty[80];
    fscanf(f, "%d %d %d %d\n", &nx, &ny, &nresx, &nresy);
    int nitems, nfiles;
    for(nfiles = 0, nitems = 0; !feof(f); nitems++, (strcmp(file_list[nfiles], "-") &&
 (file_list[nfiles][0] != '\0')) ? nfiles++ : 0/*иначе ничего*/)
        {
            fgets(file_list[nfiles], 261, f), file_list[nfiles][strlen(file_list[nfiles]) - 1] = '\0';
            if(strcmp(file_list[nfiles], "-"))
                {
                    fscanf(f, "%d %d\n", &stx[nfiles], &sty[nfiles]);
                }
        }

    //nitems--;//  |---+----\_______.    КОСТЫЛЬ  
    //nfiles--;//  |---+----/            КОСТЫЛЬ
```
   
   
 Что тут происходит конкретно, я уже и сам толком не помню. Предлагаю попробовать догадаться самостоятельно. f -- текстовый файл, откуда что-то считывается.   
   
 Интересно тут следующее:   
 1. Заголовок цикла for содержит не только  [операцию "запятая"](Операция%20запятая%20в%20Си%20Операция%20Факап)  в инициализаторе, но и в повторяемом выражении (третья часть).   
 2. Первый оператор цикла также содержит в себе операцию "запятая".   
 3. Комментарий с костылём смешной. Но почему-то закомментирован.   
 4. PVS Studio ругался не на стиль, а на то, что я условное выражение в операции ?: не обернул в скобки (хотя функционал от этого не меняется, т.к. у && приоритет выше, чем у ? ![:)](http://static.diary.ru/picture/3.gif) .   
   
 Зачем же были нужны операции запятая в таком количестве? Она тут только ухудшает читаемость. У меня только одна гипотеза. Изначально в теле цикла был только первый оператор. Когда ситуация усложнилась, я не захотел вводить новый блок, поэтому использовал всевозможные извращения, ТОЛЬКО БЫ обойтись одним оператором в теле цикла. Но в итоге сдался. А то, что уже запихнул в заголовок и в операцию "запятая", доставать обратно не стал. Эх...   
  
<https://diary.ru/~zHz00/p213309766_untitled-492.htm>  
  
Теги:  
[[Программирование]]  
[[Говнокод]]  
ID: p213309766  


(Комментариев нет)
------------------