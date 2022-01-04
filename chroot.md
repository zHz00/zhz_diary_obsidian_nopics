chroot
=======

   
 2020-08-01, 23:59   
  У микроконтроллеров туго с энергонезависимой памятью. Если надо хранить настройки, то приходится это делать обычно в основной флешке микроконтроллера. Встаёт вопрос -- где и как в этой флешке их хранить. Но это тема для отдельной статьи. Часто советуют просто писать куда-нибудь в конец флешки, где почти наверняка никаких данных не будет. Это методически неправильно, конечно, но работает.   
   
 Обычно система такая. Прошивка грузится и считывает зону настроек. Если она определяет, что зона повреждена или пустая -- грузятся настройки по умолчанию (и сразу сохраняются в зону). Если зона в порядке, настройки берутся оттуда.   
   
 Вы сделали прошивку с настройками по умолчанию. Прошиваете -- а они не применяются. Несколько раз меняете настройки, а они всё равно не применяются. В чём дело?   
   
 В том, что настройки считываются из конца флешки. Да, вы осуществили перепрошивку, но прошивальщик затёр не всю флешку, а только те куски, которые он собирается прошивать. В нормальной ситуации это бы дало хороший результат, но не в нашей, когда в конце флешки нелегально тусуются настройки. Поскольку эта зона по мнению прошивальщика/линкера свободная, то и незачем её чистить.   
   
 Иногда в среде разработки есть настройка "очищать флеш-память микроконтроллера полностью". Если её поставить -- проблема пропадёт. Но если такой настройки нет, то придётся перед каждой прошивкой вручную чистить память при помощи отдельной утилиты.   
    
 <https://diary.ru/~zHz00/p219749546_chroot.htm>   
   
 Теги:   
 [[Программирование]]   
 [[Борьба с техникой]]   
 ID: p219749546