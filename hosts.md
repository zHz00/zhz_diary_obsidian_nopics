hosts
=====

  
2012-11-24, 22:50  
 Стал замечать, что некоторые html-страницы не могу сохранить, браузер говорит что не может прочитать один из файлов. Файл называется ga.js . Я тогда ещё не знал, что это. Стал искать возможность пропускать файлы при сохранении. Потом стал искать что это за ga.js . Оказалось, google-analytics. O\_O. Почему не может открыть-то? Потому что в  [hosts](https://ru.wikipedia.org/wiki/Hosts)  написано  [www.google-analytics.com](https://www.google-analytics.com)  127.0.0.1 . Не, это правильно. Незачем гуглу знать, что у меня происходит. Но кто туда это написал? Не я. Антивирусов не держу. По этому поводу я остался в раздумьях.   
   
 Но сохранять страницы-то хочется (не копируя их в ворд -- это извращение)! А раскрывать посещённые страницы нет. Делаю на компьютере http-сервер и размещаю на нём ga.js, пустой! Увы, это не сработало, т.к. доступ к гугл-аналитикс осуществляется по https!   
   
 С нахрапу сделать сервер с SSL почему-то не удалось. Сработает ли такой трюк или самоподписанный сертификат отвергнется? Или спросят меня, согласен ли я, как обычно это бывает с "не совсем хорошими" сертификатами?   
   
 Борьба продолжается.   
  
<https://diary.ru/~zHz00/p182891751_hosts.htm>  
  
Теги:  
[[Борьба с техникой]]  
ID: p182891751  


Комментарии: 2
--------------

  


---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (1/2) | 2012-11-24, 23:12 | Гость | c618724369 |

  
 очень рекомендую adblock plus. даже если в него не загружать список рекламы, чтобы резал рекламу (мало ли, вдруг реклама нравится), туда можно прописать гуглоаналитику и ещё несколько сайтов счётчиков. Ибо нефиг. Они будут очень аккуратно резаться.   
   
 // ssvda   
 ^c618724369

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (2/2) | 2012-11-25, 13:39 | himself | c618763075 |

  
 Ты небось и прописал, только забыл ![:)](http://static.diary.ru/picture/3.gif)   
 ^c618763075