Peter Linden // Expert C Programming: Deep C Secrets
=====================================================

   
 2018-08-02, 23:59   
  Не-программисты могут сразу читать спойлер, там смешная история.   
   
 Наткнулся я на эту книгу, потому что стал гуглить, что означает аббревиатура BSS. Её я встретил в обозначениях областей памяти в скрипте линковщика. Секция, обозначенная BSS, предназначена для занулённых (глобальных) данных. В других местах было написано, что это значит Better Save Space, потому что эта секция занимает место только в оперативной памяти, а в исполняемом файле под неё место не выделяется. Однако оказалось, что это историческое наследие машины IBM 704, а на самом деле расшифровывается "Block Started by Symbol".   
   
 На русском языке обнаружить данную книгу не удалось. Книга очень старая, 1994 года, хотя для книг по Си это очень неплохо. Книга содержит в себе множество трюков, которые можно делать в Си, множество особенностей внутренней Си-кухни, а также некоторые советы. Всё это перемежается байками из истории компьютерной техники. В этом смысле данная книга подобна "Как не надо программировать на Си++ или почему 2+2=5986" Уэллина.   
   
 Я нашёл мало нового в этой книге. Около 80% я знал и так. Однако очень многое из того, что я знал, мне приходилось годами собирать по крупицам в интернете, в разговорах с другими специалистами, либо познавать на собственном опыте. Ни одна учебная книжка по Си не содержит такого количества нюансов. Другими словами, можно либо выяснять всё самостоятельно, потратив на это кучу времени, либо воспользоваться данной книгой.   
   
 Автор кое-что сказал и про Си++, но в основном он говорит о Си. Сейчас такие знания нужны уже не каждому. Но если вы хотите быть настоящим специалистом по Си или Си++, то всё описанное вы должны знать. Также рекомендую эту книгу интересующимся историей компьютерной техники.   
   
 В качестве примера я привожу перевод отрывка, посвящённого программе  [Элиза](https://ru.wikipedia.org/wiki/%D0%AD%D0%BB%D0%B8%D0%B7%D0%B0_%28%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B0%29)  :   
   
  [Элиза](https://zHz00.diary.ru/p215960758.htm?index=1#linkmore215960758m1)    
>   **Элиза**    
>    
>  Одной из первых программ, которая работала с естественной речью, была Элиза (Eliza). Названа она была по имени болтливой героини пьесы Пигмалион Бернарда Шоу. Элиза была написана в 1965 году профессором MIT Джозефом Вейзенбаумом (Joseph Weizenbaum). Она изображала поведение психотерапевта роджерского подхода при разговоре с пациентом. Программа делала поверхностный анализ вводимого текста и выдавала один из множества встроенных ответов. Создавая иллюзию того, что компьютер понимает всё, что ему говорят, эта программа обдурила множество далёких от компьютеров людей.   
>    
>  Впервые Вейзенбаум столкнулся с этим феноменом, когда пригласил свою секретаршу на испытание. Спустя пару минут диалога с Элизой секретарша (которая несколько месяцев наблюдала за тем, как Вейзенбаум разрабатывает ПО, и которая должна была лучше всех понимать, что это просто программа) попросила Вейзенбаума выйти, чтобы она могла продолжить разговор с глазу на глаз.   
>    
>  Тест Тьюринга провалился при первой же попытке, поскольку секретарша сочла человеком простую программу, не претендующую на искусственный интеллект. В этом случае не программа проявила интеллект, а человек проявил доверчивость. Элиза оказалась популярной программой. Бостонская компьютерная консалтинговая компания BBN (Bolt, Berenek and Newman) её переделала. Результатом стал более серьёзный тест, при котором вице-президент BBN обнаружил, что он разговаривает с Элизой, а не с программистом, с которым он собирался говорить изначально.   
>    
>   **Элиза против вице-президента (ВП)**    
>    
>  Даниэль Боборов (Daniel Bobrow), учёный в области компьютерных технологий, поведал об этом случае в декабрьском выпуске журнала SIGART Newsletter за 1968 год.   
>    
>  Одним субботним утром, в районе 9 часов утра, ВП, ответственный за продажи наших услуг Telecomp, явился в наш машинный зал. Он хотел воспользоваться компьютером для проведения демонснтрации. Рядом с терминалом он обнаружил записку, гласившую: "печатайте все сообщения на терминале, в конце ставьте точку". Записку оставил мимопроходящий программист, которому только что удалось запустить Lisp-версию программы Доктор (в основе которой лежала Элиза за автроством Вейзенбаума). Однако, ВП решил, что эта записка означает, что терминал подсоединён к другому, расположенному у меня дома, поскольку я часто использовал компьютер всю ночь. Поэтому он начал печатать сообщения "мне", но на самом деле он обращался к программе Доктор. Произошёл следующий диалог:   
>    
>  ВП: Можно мне воспользоваться компьютером сегодня утром?   
>  Терминал: Почему вы спрашиваете?   
>  ВП: Думаю, у меня может получиться увеличить продажи наших Telecomp-услуг.   
>  Терминал: Почему вы сомневаетесь?   
>  ВП: Предполагаемые клиенты никогда не видели нашу систему в действии.   
>  Терминал: Это важно для вас?   
>  ВП: Конечно.   
>  Терминал: Я вижу, вы в этом уверены.   
>  ВП: Это возмутительно!   
>  Терминал: Что вы имеете ввиду под "это возмутительно"?   
>  ВП: Есть причина, по которой я не могу воспользоваться компьютером сейчас?   
>  Терминал: Расскажите мне больше о ваших предполагаемых клиентах.   
>  ВП: Пожалуйста, позвоните мне   
>    
>  Заметьте, что после этой фразы ВП не выполнил инструкцию и не поставил точку. Поэтому компьютер не ответил ему. Это так разозлило ВП, который думал, что я издеваюсь над ним, что он позвонил мне сам, пробудив от глубокого сна, и сказал:   
>    
>  ВП: Почему вы так мерзко себя со мной ведёте?   
>  Я: Что вы имеете ввиду под "почему вы так мерзко себя со мной ведёте"?   
>    
>  ВП со злостью прочитал мне диалог, который у "нас" только что состоялся. От меня он услышал только хохот. Ушло некоторое время на то, чтобы убедить его, что он разговаривал с компьютером.   
>    
>  Обратите внимание, что Элиза/Доктор как правило отвечает открытым вопросом. Это сделано для того, чтобы пользователь написал больше предложений. Она не пытается отвечать на вопросы, потому что не умеет. Несмотря на это, программа смогла обдурить вице-президента, который был компьютерно грамотным. Причина, по которой обман удался (и программа прошла тест Тьюринга) вовсе не в том, что у программы есть интеллект. Хотя в своё время такие программы были в диковинку, на текущий момент она кажется весьма простой. Она обманывает людей, потому что людей просто обмануть. Поэтому такое тестирование бессмысленно. Вторая попытка провести тест Тьюринга тоже провалилась. 

     
    
 <https://diary.ru/~zHz00/p215960758_peter-linden-expert-c-programming-deep-c-secrets.htm>   
   
 Теги:   
 [[Переводы]]   
 [[Книги]]   
 ID: p215960758