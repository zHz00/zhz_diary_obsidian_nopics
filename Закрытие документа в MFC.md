Закрытие документа в MFC
========================

  
2012-10-09, 22:45  
 Ну, MFC это поделие. Используйте Qt.   
   
 Итак, надо закрыть документ в MDI приложении MFC. Как это сделать?   
 Всё не так просто. Допустим, CDocument1 -- класс, производный от CDocument. Если вы создали об'ект как:   
   
 CDocument1 \*pDoc=new CDocument1();   
   
 Потом наполнили его содержимым (без создания вида -- класса, производного от CView), то удалять его надо так:   
   
 delete []pDoc;   
   
 А если вы его создали так:   
   
 CDocument1 \*pDoc=OpenDocumentFile("Filename.ext");   
 где OpenDocumentFile -- член классов, производных от CWinApp или CDocumentTemplate.   
   
 То удалять надо так:   
   
 pDoc->OnCloseDocument();   
   
 Вопрос, можно ли удалять так документы, к которым вручную прицеплен вид (CView) -- открыт. Нарушение порядка удаления приведёт либо к утечке памяти, либо к вылету программы.   
  
<https://diary.ru/~zHz00/p181457344_zakrytie-dokumenta-v-mfc.htm>  
  
Теги:  
[[Программирование]]  
ID: p181457344  


(Комментариев нет)
------------------