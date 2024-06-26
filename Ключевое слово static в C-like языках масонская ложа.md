Ключевое слово static в C-like языках: масонская ложа
=====================================================

  
2017-10-23, 23:58  
 На эту тему существует несколько заметок, однако я счёл, что все они обладают недостатками. Поэтому решил написать свою.   
   
 Ключевое слово static -- наверное, самое эзотерическое слово в языках Си-группы. Во-первых, оно довольно редко нужно (но volatile, конечно, круче). Во-вторых, в зависимости от контекста это слово может означать довольно разные вещи.   
   
  [(читать дальше)](https://zHz00.diary.ru/p213990567.htm?index=1#linkmore213990567m1)    Если пытаться найти что-то общее среди всех применений, то static следует переводить как "глобальный, но скрывает это".   
   
  **1 Язык Си**    
   
  **1.1 Статическая локальная переменная**    
   
 С этим применением сталкивались многие. Как известно, локальные переменные теряют свои значения после завершения функции. Каждый раз функция начинает работать с чистого (а точнее, с грязного) листа. Если же локальную переменную об'явить как статическую, она будет начинать с тем значением, на котором закончила в предыдущий раз. Даже если присутствует инициализация, та будет выполняться только один раз. При первом вызове (а на самом деле -- при старте программы).   
   
  **Пример.**  
```
void f(void)  
	{
		static int n=0;
		printf("n=%d\n",n);
		n++;
	}

int main (void)
	{
		f();
		f();
		f();  
	}
```
 На экране должно появиться:   
   
 0   
 1   
 2   
   
  **Зачем.**  В данном случае переменная просто является счётчиком вызовов (что может быть полезно). Основное же применение статической локальной переменной -- это сохранение состояния вычислений внутри функции. Имеет смысл в различных итерационных методах, когда каждый вызов функции должен учитывать результаты работы предыдущих. Например, так можно реализовать конечный автомат.   
   
 Для сохранения состояния придётся делать одно из трёх. Либо нам придётся хранить состояние в глобальных переменных (а это всегда методически плохо), либо каждый раз его возвращать из функции, чтобы получить при следующем вызове (потеря времени), либо вот это.   
   
 Как известно, данные программа размещает в трёх местах. Во-первых, это стек. Там хранятся все локальные переменные. Во-вторых, это так называемая "область данных". Некая область, куда при старте программы складываются все глобальные переменные. Туда же попадают, к примеру, строковые константы. В примере выше строка "n=%d\n" будет храниться в области данных. Третье место -- это динамическая память. То, что получается через malloc/new. Эта память выделяется на ходу и не имеет отношения к двум предыдущим.   
   
 Ясное дело, что статическая переменная не может располагаться в динамической памяти, т.к. мы не вызывали malloc. На стеке ей тоже не место, т.к. содержимое стека постоянно гуляет. После завершения функции все данные этой функции заменяются новыми, которые туда записывают другие функции (именно поэтому значения не сохраняются). Да, статические локальные переменные хранятся в области данных. Там же, где глобальные переменные. Ими они, по сути, и являются. Просто видны исключительно внутри конкретной функции.   
   
  **1.2 Статическая глобальная переменная**    
   
 Это вообще отдельная песня. Если глобальная переменная об'явлена как статическая, она видна только в текущем c-файле (модуле; дальше для краткости я буду писать именно "модуль", а не "единица трансляции" и не "c-файл").   
   
  **Пример.**    
   
 file1.c: 
```
static int i;//1
```
 file2.c: 
```
int i;//2
```
 file3.c: 
```
extern int i;
```
 К глобальным переменным нельзя просто так получить доступ из других модулей. Надо в этих других модулях об'явить переменную как extern. Тогда при сборке компоновщик (linker) будет искать переменную с таким именем в других модулях. Однако если переменная статическая, компоновщик к ней не привяжется, а будет искать другую. В данном случае в file3.c будет использоваться переменная, у которой в комментарии указано "2". (Переменные, обозначенные "1" и "2" -- разные.)   
   
  **Зачем.**  Чтобы у вас не возникло конфликта с каким-нибудь ушлёпком, который в соседнем модуле сделал глобальную переменную с таким же именем.   
   
  **1.3 Статическая (глобальная) функция**    
   
 А неглобальных функций в Си и нет. Тут то же, что и глобальная переменная, но есть особенность.   
   
 По-умолчанию, все функции автоматически видны из других модулей. Если в модуле отсутствует  **функция**  , при сборке автоматически проверяются все остальные модули. Если же в текущем модуле нету необходимой  **переменной**  , программа соберётся только если в модуле, где переменной нет, вместо переменной присутствует указание extern (как в примере выше). Обращаю внимание, что об'явление переменной как extern -- "ненастоящее". Оно не резервирует память. Оно просто указывает, что такая переменная есть где-то ещё.   
   
 Это несколько влияет на те случаи, когда функцию надо об'являть статической.   
   
 Так или иначе, статическая функция не сможет быть вызвана из других модулей, кроме текущего. Либо будет вызвана одноимённая ещё из какого-нибудь модуля, либо программа не соберётся вообще.   
   
  **2 C++**    
   
 То, что работало в Си, точно так же работает и в Си++, но тут на это ключевое слово навесили ещё кое-что.   
   
 В качестве предупреждения о разнице между Си и Си++ указываю на то, что лично для меня оказалось неожиданностью: локальная статическая переменная метода класса существует в единственном экземпляре на  *все экземпляры*  класса.   
   
  **2.1 Статическое поле класса**    
   
 Статическое поле (переменная-член) класса существует в единственном экземпляре на все экземпляры класса. И хранится оно, как вы уже догадываетесь, в области глобальных переменных.   
   
  **Пример.**  
```
struct A  
	{  
		static int i;  
		int a;  
		A(){a=0;}  
	};  
  
int A::i=5;//*  
  
int main(void)  
	{  
		A a,b;  
		a.a=7;  
		a.i=8;  
  
		printf("b.a=%d\n",b.a,"b.i=%d\n",b.i);  
	}
```
 Мы увидим:   
 0   
 8   
   
 Хотя 8 мы записывали только в об'ект a!   
   
 Обратите внимание на строчку, помеченную звёздочкой. Дело в том, что об'явление статического поля класса не производит выделения памяти под него в области данных. На самом деле это странно, но вот так. Поэтому надо выбрать модуль, в котором реально будет храниться это поле, и сделать там его настоящее определение так, как это указано выше (при этом об'являть как extern в других модулях поле не надо!). Там же можно его инициализировать (и это единственный способ). В конструкторах "инициализировать" статические поля не советую, т.к. при создании каждого об'екта значение поля будет перезаписываться.   
   
 Также можно получать доступ к статическому полю иным способом:   
 
```
A::i=9;
```
 При этом существование хотя бы одного экземпляра класса необязательно.   
   
 Я пишу про классы, но почему-то в примере у меня структура. Почему я так сделал -- предлагаю разобрать самостоятельно. Вместо этого я укажу на совершенно неожиданное свойство, касающееся об'единений (union). Оказывается, поля об'единения не могут быть статическими, кроме одного очень особого случая. Если об'единение у вас анонимное и глобальное, то все его поля должны быть явно об'явлены как static.  [Пруф.](https://msdn.microsoft.com/en-us/library/y5f6w579.aspx)    
   
  **Зачем.**  Я могу придумать несколько применений:   
 1. Общие данные класса. Например, счётчик экземпляров. Можно в каждом конструкторе увеличивать поле на 1, тогда его значение будет показывать число созданных экземпляров. Для корректной работы требуется создание отдельного конструктора копий, иначе может возникнуть казус.   
 2. Общие ресурсы класса. Если все экземпляры класса работают с каким-либо оборудованием, то через статические поля можно реализовать работу с мьютексом (взаимоисключением).   
 3. Данные, предназначенные специально для работы со статическими методами (см. ниже).   
   
 Статическое поле класса по сути является глобальной переменной, но об этом знает только класс, полем которого оно является.   
   
  **2.2 Статический метод класса**    
   
 Чем на низком уровне отличается метод класса от простой функции? Наличием скрытого параметра. Вы никогда не задумывались о том, как метод класса определяет, у какого экземпляра его вызвали? Может быть, на каждый экземпляр создаётся отдельная функция, в которой прибиты адреса полей к полям конкретного экземпляра класса?   
   
 Нет.   
   
 Метод класса получает ещё один, невидимый параметр. И это указатель this. Все обращения к полям внутри метода осуществляются через него. Мы пишем: 
```
void A::f(void)  
	{  
		a=3;  
	}  
//...  
A a;  
a.f();
```
 А на самом деле это (я по ассемблеру проверил): 
```
void f(A *this)  
	{  
		this->a=3;  
	}  
//...  
A a;  
f(&a);
```
 (естественно в жизни это не заработает, т.к. компилятор не разрешит использовать this как имя переменной; для проверки я использовал имя \_this)   
   
 Наличие скрытого параметра накладывает ряд ограничений на метод класса. Например, некоторые функции стандартной библиотеки, WinAPI и т.п. от нас хотят получить в качестве аргумента указатель на функцию (например, функции qsort, CreateThread). Однако даже при совпадении прототипа мы не можем передать указатель на метод класса, т.к. на самом-то деле прототип не совпадает! Есть только видимость совпадения. И как бы мы прототип не меняли, мы не сможем избавиться от скрытого параметра.   
   
 Если не об'явим метод статическим.   
   
 Статический метод не получает указателя this. Таким образом, он может обращаться напрямую только к статическим полям класса. Чтобы получить доступ к остальным, ему надо как-то (например, через параметры) получить указатель на какой-нибудь экземпляр класса.   
   
 Конструктор и деструктор статическими быть не могут (в C# конструктор статическим быть может, см. далее).   
   
  **Пример.**  CreateThread -- отличный пример. 
```
struct A  
	{  
		int i;  
		static DWORD ThreadFunc(LPVOID _this)  
			{  
				for( ; ; )  
					{  
						((A*)_this)->i++;  
						Sleep(1);  
					}  
			}  
	};  
//...  
A a,b,c;  
CreateThread(NULL,0,ThreadFunc,(void*)&a,0,NULL);  
CreateThread(NULL,0,ThreadFunc,(void*)&b,0,NULL);  
CreateThread(NULL,0,ThreadFunc,(void*)&c,0,NULL);
```
 CreateThread создаёт новый поток программы, который выполняется  *одновременно*  с main. В качестве третьего параметра функция хочет получить указатель на функцию со следующим прототипом: 
```
DWORD ThreadFunc(LPVOID *p);
```
 Возвращаемое значение нужно как результат работы функции. А указатель в качестве параметра (LPVOID это то же, что и void\*) позволяет функции потока понять, с какими данными работать.   
   
 Четвёртый параметр CreateThread является тем самым указателем, который потом передаётся функции потока. (остальные параметры в данном случае значения не имеют)   
   
 Таким образом, статическая функция вызывается трижды и каждый раз при вызове получает указатель на свой об'ект класса A. В данном случае эта функция ничего особенного не делает, просто увеличивает значение поля класса. В приведённом выше примере значения полей каждого из трёх экземпляров будут расти синхронно.   
   
  **Зачем.**    
 1. Пример выше: мы хотим передать указатель на метод класса как аргумент другой функции.   
 2. Метод не зависит от данных класса. Типичный пример этого -- математические библиотеки. Создаётся класс без полей, но с кучей статических методов, которые делают сложные расчёты. Экземпляра класса может даже не быть, а методы могут вызываться через имя\_класса::метод(). Преимущество тут в том, что нам не надо создавать отдельный экземпляр класса при вызове. И не надо думать, имеется ли сейчас в области видимости хотя бы один. Хотя я считаю, что для этих целей лучше подходят пространства имён (namespace).   
 3. Метод требует статических данных подобных статической локальной переменной, однако необходимо, чтобы экземпляры класса имели к этим данным доступ. Это как раз тот случай, о котором я писал в п. 2.1, когда статические поля класса создаются по нужде статического метода.   
   
  **3 С# и Java**    
   
 Поскольку я не знаю этих языков, в тексте ниже могут быть неточности. Информация ниже почёрпнута из открытых источников. Буду благодарен за исправления и дополнения.   
   
  **3.1 Java: статический вложенный класс**    
   
 В Яве нету:   
 * глобальных переменных (а значит, и их статичности);
* глобальных функций (а значит, и их статичности);
* статических локальных переменных.

   
 Однако статические методы и поля классов работают точно так же.   
   
 Появляется одна новая фишка -- статический вложенный класс. (подробности  [тут](https://stackoverflow.com/questions/7486012/static-classes-in-java)  )   
   
 Оказывается, чтобы создать экземпляр вложенного класса, сначала надо создать экземпляр внешнего класса: 
```
public class A  
	{  
		public class B  
			{  
			}  
	}  
  
A a= new A();  
B b = a.new B();
```
 Однако если вложенный класс статический, то можно делать так: 
```
public class A  
	{  
		public static class B  
			{  
			}  
	}  
  
B b = new B();
```
  **3.2 C#: статический класс и статический конструктор**    
   
 Глобальных переменных и функций тоже нет. Статические локальные переменные поддерживаются. Статические поля и методы классов тоже.   
   
 Тут новая фишка называется почти так же, а значит совсем другое. Тут просто "статический класс". 
```
public static class A  
	{  
	}
```
 Статический класс -- это класс, все методы и поля которого должны быть статическими. При этом нельзя создать ни одного экземпляра данного класса. По-видимому это костыль, заменяющий глобальные переменные.   
   
 Кроме того, поддерживается статический конструктор (не поддерживаемый в Яве и Си++), предназначенный для инициализации статических полей. Указано, что он вызывается в промежутке между стартом программы и созданием первого экземпляра класса. По-видимому, использовать статические поля без хотя бы одного экземпляра нельзя, иначе когда будет вызываться конструктор?   
   
  **4 PHP: позднее статическое связывание**    
   
 Статически локальные переменные работают в PHP примерно так же. Статические поля класса и методы тоже (но статическое поле нельзя указать через стрелку, только через два двоеточия). Но всё же слово static явно мёдом намазано, поэтому в PHP на нём висит ещё одно совершенно особенное действие: static, подобно self, раскрывается как имя класса. но не того класса, где слово static написано (как это происходит с self), а того, в котором реально происходит вызов. Это называется позднее статическое связывание (late static binding, подробнее об этом  [тут](http://php.net/manual/en/language.oop5.late-static-bindings.php)  ).   
   
  **Пример.**  ( отсюда:  [php.net/manual/en/language.oop5.static.php#1048...](http://php.net/manual/en/language.oop5.static.php#104823)  ) 
```
  
class a  
	{  
		static protected $test="class a";  
		public function static_test()  
			{  
				echo static::$test."\n"; // Results class b  
				echo self::$test."\n"; // Results class a  
			}  
  
	}  
  
class b extends a  
	{  
		static protected $test="class b";  
	}  
  
$obj = new b();  
$obj->static_test();
```
 Должно вывести:   
 class b   
 class a   
   
  **5 Заключение**    
   
 А ну его, устал я...   
     
  
<https://diary.ru/~zHz00/p213990567_klyuchevoe-slovo-static-v-c-like-yazykah-masonskaya-lozha.htm>  
  
Теги:  
[[Программирование]]  
[[Статьи]]  
ID: p213990567  


Комментарии: 5
--------------

  


---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (1/5) | 2020-03-03, 00:15 | Гость | c745888281 |

  
 Суперская статья   
 ^c745888281

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (2/5) | 2020-03-03, 00:16 | Гость | c745888284 |

  
 Круто   
 ^c745888284

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (3/5) | 2020-03-03, 00:20 | zHz00 | c745888337 |

  
 Спасибо ![:D](http://static.diary.ru/picture/1131.gif)   
 ^c745888337

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (4/5) | 2020-04-26, 18:16 | Гость | c746834204 |

  
 Мегасасибо!   
 ^c746834204

---



|         #         |              Дата              |                     Автор                     |           ID           |
| --- | --- | --- | --- |
| (5/5) | 2020-05-15, 19:06 | Гость | c747186766 |

  
 Good   
 ^c747186766