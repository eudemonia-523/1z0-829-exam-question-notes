# 第一章 Building Blocks

## 1.1 Content and notes

1. 这里的Building Blocks就是指的代码块比如 Class{ } Record{ } Interface{ } Enum{ }
2. 一个java文件最多只有一个public类，但是没有public类也是完全可以的，那么文件名最好与其中一个类同名，但是不同名也是没关系的。但是只要有public类就必须与文件名相同
3. 编译和运行java文件：编译比如：javac myClass.java 运行java类并传入两个参数比如：java myClass "param1" "param2" 如果需要编译的文件不在当前目录可以用-d指定路径比如：javac -d classes packagea/ClassA.java 运行的时候java -cp classes packageb.ClassB 注意-cp等效-classpath 和 --class-path
4. 一般的import比如：import java.lang.Math, 你也可以选择static import去引入静态方法比如：import static java.lang.Math.sqrt
5. 在import的时候使用通配符 * 要记住 * 只会引入当前包的所有类不会递归引入，而且 * 永远只有一个且放在最末尾
6. 常见使用：jar -cvf myNewFile.jar . 的三个符号分别代表 1.创建jar文件 2.显示详细输出3.指定生成文件名。最后的 . 说的是生成文件的目录，如果加上-Ｃ 就意味着你想生成的myNewFile.jar不要在当前目录而是 -Ｃ指定的目录
7. 使用package必须在第一行，import必须跟随在他后面
8. 构造方法名字与public类相同而且无返回值
9. 初始化顺序：父类静态字段->父类静态块->子类静态字段->子类静态块->父类实例字段->父类实例块->父类构造器->子类实例字段->子类实例块->子类构造器 (先执行块代码然后执行构造器，代码块和变量的优先级相同，先后顺序按照代码的先后顺序进行)
10. 八种基本数据类型中注意short和char因为他们都是16位但是short有正负值char只有正值
11. java命名只有字母数字下划线和货币符号（美元欧元人民币都可以），其中数字不能做首字母其他的都可以。注意不要使用保留字作为名称
12. final修饰的值不能修改，如果是引用变量则引用不能改，但是引用指向的内容可以改。变量没初始化或者可能不会被初始化的时候使用会不通过编译
13. var在声明的时候就要确定类型，如果刚开始不确定类型但是在使用的时候说明了只能是int 一样通不过编译，放在参数列表无法编译因为不知道类型
14. 访问修饰符比较灵活 public / static / final 顺序可以任意组合，但是均需放在返回值前面，范型参数<T> 也要放在返回值前面，throws放在最后面
15. 文本块起始的"""必须立刻接换行否则编译报错但结束的则不需要单独一行，计算长度的时候要考虑每次换行多了一个/n也要计算在内。计算长度以最小缩进作为起始点。
16. 初始化语句比如：int i1, i2, i3 = 0; 只有i3是初始化过的，只有同一类型可以放在同一个分号内
17. 实例变量的scope在实例可以被垃圾回收的时候终止，类变量只在程序结束的时候终止

## 1.2 Topics

### 1.2.1 自动装箱拆箱

1. 装箱和拆箱发生在原始类型和包装类之间，注意String是无法自动装箱拆箱的，它不是原始类型
2. 算数运算会触发自动拆箱，比如Integer和int相加的时候先转换再相加
3. 方法重载优先精确匹配，Integer能匹配上就不会用Object
4. 数组初始化的时候无法装箱拆箱，如果你期待它自动进行是会报错的
5. List&lt;int&gt; 是非法的，范型无法是原始类型
6. char和short的转换需要cast ，其他的小转大都不需要cast因为大的可以容纳小的
7. array有length属性而不是length方法
8. array有重写了clone（）方法，你可以用它生成新的array但是只会克隆结构不会克隆里面的所有内容
9. float最大大概是1234567, 如果12345678就会超长了，1.234567f也是一样，超长会损失精度
10. double不需要像float一样在后面加f double d=1.2 是合法的，他的长度大约是15-16位有效数字。 float f = 3.14 会出错，把一个double不经过处理传给了float
11. char 跟short 之间如果不强转是无法通过编译的
12. ？： 的第二个和第三个不允许是对一个void方法的调用，因为运算符的存在就是为了赋值，返回一个void很明显有问题

### 1.2.2 基本数据类型和它们的包装类

| Keyword | Type                     | Min Value      | Max Value     | Default Value | Example |
| ------- | ------------------------ | -------------- | ------------- | ------------- | ------- |
| boolean | true or false            | n/a            | n/a           | false         | true    |
| byte    | 8-bit integral value     | -128           | 127           | 0             | 123     |
| short   | 16-bit integral value    | -32,768        | 32,767        | 0             | 123     |
| int     | 32-bit integral value    | -2,147,483,648 | 2,147,483,647 | 0             | 123     |
| long    | 64-bit integral value    | -2⁶³           | 2⁶³ – 1       | 0L            | 123L    |
| float   | 32-bit floating-point    | n/a            | n/a           | 0.0f          | 123.45f |
| double  | 64-bit floating-point    | n/a            | n/a           | 0.0           | 123.456 |
| char    | 16-bit Unicode character | 0              | 65,535        | '\u0000'      | 'a'     |

| Primitive Type | Wrapper Class | Inherits from `Number`? | Example of Creating          |
| -------------- | ------------- | ----------------------- | ---------------------------- |
| boolean        | Boolean       | No                      | `Boolean.valueOf(true)`      |
| byte           | Byte          | Yes                     | `Byte.valueOf((byte) 1)`     |
| short          | Short         | Yes                     | `Short.valueOf((short) 1)`   |
| int            | Integer       | Yes                     | `Integer.valueOf(1)`         |
| long           | Long          | Yes                     | `Long.valueOf(1)`            |
| float          | Float         | Yes                     | `Float.valueOf((float) 1.0)` |
| double         | Double        | Yes                     | `Double.valueOf(1.0)`        |
| char           | Character     | No                      | `Character.valueOf('c')`     |

# 第二章 Operators

## 2.1 Content and notes

1. 如果两个值具有不同的数据类型，Java 会自动提升其中一个值为两种数据类型中较大的一个
2. 如果其中一个值是整数，另一个值是浮点数，则 Java 会自动将整数值提升为浮点值的数据类型
3. 较小的数据类型，即byte、 short和char，在与带有变量（而不是值）的 Java 二进制算术运算符一起使用时，首先会提升为int ，即使两个操作数都不是int 
4. 所有提升均已完成，且操作数具有相同的数据类型后，结果值将具有与其提升的操作数相同的数据类型。运算的时候大类型不转换直接赋值给小类型会报错
5. 这一章做题的时候容易后陷阱 要注意是否能通过编译比如float y = 2.1; 就无法编译 这里的自动类型转换要多做点题
6. 注意-不能用在boolean，！不能用在数值。比如：int p = !5; boolean s = !0; 都是无法编译的
7. instanceof运算符在使用时，如果两个类没有任何关系那么编译不会通过，如果两个类有继承关系就会通过，大不了在运行的时候碰到不对的类型报异常

## 2.2 Topics

### 2.2.1 常见运算符

| **类别** | **常考运算符（部分）**                            | **含义**                                                     |
| -------- | ------------------------------------------------- | ------------------------------------------------------------ |
| 算术     | +, -, \*, /, %                                    | 加减乘除取余数                                               |
| 赋值     | +=, -=, \*=, /=, %=  &=  ^=  &lt;<=  &gt;>=  >>>= | 按位与赋值，按位异或赋值，左移赋值， 右移赋值， 无符号右移赋值 |
| 逻辑     | &&,  \|\|                                         | 逻辑与 和 逻辑或的                                           |
| 比较     | ==, !=, &lt;, &gt;, &lt;=, &gt;=                  | 判断                                                         |
| 位运算   | &,  \|                                            | 按位与 和 按位或的运算， ~是按位取反，^是按位异或            |
| 自增自减 | ++, --                                            | 自增自减                                                     |
| 条件三元 | ?:                                                | 三元判断                                                     |
| 其他     | instanceof                                        | 注意instance是operator！                                     |

# 第三章 Making Decisions

## 3.1 Content and notes

1. if和else后面的{}都可以省略，但是推荐做法还是要写{}，可读性更高，如果不写{} 那么if else 都会只对第一条起作用

2. if 里的判断语句无法使用数字 0/1，在其他语言里也许可以但是java不行

3. 语句 如果没有break;会从上到下依次执行，switch是可以不包括任何case的，也可以没有default。switch的case里的值必须是编译时期就能确定的常量 比如字面量 枚举 或者 final constant 并且case 变量和switch变量必须是同一类型。 （注意非final变量会导致编译失败）

4.  模式匹配，可用于if/switch 不可用于while。而且新创建的对象只在true的代码块中起作用，右边必须是左边的子类，同类都不行，比如：
   ```java
   static void test(Number number) {
   	if (number instanceof Integer i) {
   		System.out.println(i + 10);
   	}
   }
   static void handle(Object obj) {
   	switch (obj) {
   		case String s -> System.out.println("String: " + s);
   		case Integer i -> System.out.println("Integer: " + i);
   		default -> System.out.println("Something else");
   	}
   }
   //关于scope，如果表达式可以明确的判断是否为真，那么有时超出scope的使用仍然可行
   ```

5. switch表达式：使用->而不是：，不用break，多行表达式可以用yield返回，switch变量的值必须被全部遍历，否则就要添加default分支。记得最后的分号，没有的话报错。返回值需要和声明的一致，不用完全一直但是至少要能自动转换。留意所有的分号

6. 普通for循环，初始化的变量必须是同一个类型，否则编译报错

7. 增强 for-each 是用来对数组和各种集合进行遍历的，此处的集合指的是实现了Iterable的类,没有实现的集合无法用此方法进行遍历

8. for / while / do while 都可以用可选label， break label 和continue label 是用于跨循环操作的场景，因为如果是最近的一层根本不需要label是默认行为 比如：
   ```java
   int[][] myComplexArray = {{5,2,1,3},{3,9,8,9},{5,7,12,7}}; 
   OUTER_LOOP: for(int[] mySimpleArray : myComplexArray) {
       INNER_LOOP: for(int i=0; i<mySimpleArray.length; i++) {
           System.out.print(mySimpleArray[i]+"\t"); 
       }
    	System.out.println();
   }
   ```

9. break可以出现在switch while do 和 for， continue也可以而且它还可以出现在if语句来跳过if的后续代码直接退出

10. break是用来完全跳出当前循环的，而不是跳过当次循环进入下一次循环，这是continue的用法

11. switch表达式不可以返回空，但是switch语句的分支可以，表达式要么返回一个值要么抛异常，返回值需要和声明的一致，不用完全一直但是至少要能自动转换。留意所有的分号。switch表达式如果没有列举所有可能的case就必须要一个default，简而言之必须能成功返回一个值
    ```java
    String result = switch (day) {
    	case 1,3,4 -> "Mon";
    	case 2 -> {
    		System.out.println("Calculating...");
    		yield "Tue"; // yield 替代 return，表示产生一个值
    	}
    	default -> "Other";
    };
    ```

# 第四章 Core APIs

## 4.1Content and notes

1. String的连接 String s =1+2+three+four. 如果three 是int 3, four是string4 那么答案是64前面用加法后面用连接，从左到右逐步运算和转化

2. 常见string方法：
   ```java
   myString.length()//返回字符串长度
   myString.charAt(0)//返回index所在的字符
   myString.indexOf(char or string, fromIndex or subString or subString, fromIndex) //返回索引位置
   myString.subString(int beginIndex or int beginIndex, int endIndex) //返回子串 左闭右开
   myString.toUpperCase() myString.toLowerCase
   myString.equals() myString.equalsIgnoreCase() //如果equals相等那么hashCode相同，反之不然
   myString.startsWith() myString.endsWith() myString.contains()
   myString.replace(charSequence target, charSequence replacement) //前面是我想去掉的，后面是我想替换成为的
   myString.strip() myString.trim() //其中strip功能更强支持Unicode
   myString.indent(number) myString.stripIndent() //正数和负数决定是新增缩进还是减少缩进
   myString.translateEscapes() //转换\\t这样的符号，打印结果可能有tab 或者换行之类的
   myString.isEmpty() myString.isBlank() // 区别，如果myString是" "， 那么isEmpty为false但是isBlank为true
   myString.formatted("dd%s",str) 也可以用静态方法String.format("dd%s",str)其中%s任何类型%d数字%f浮点型%n间隔符
   ```

3. StringBuilder是可变的，指向同一个起始内存空间，所以在两个StringBuilder转化的时候可能会出考题，new StringBuilder 可以指定初始化的值也可以指定大小。其他方法：
   ```java
   var sb = new StringBuilder("abcde");
   sb.indexOf("a");
   sb.append("ab");
   sb.length();
   sb.charAt(5);
   sb.subString();//这个方法回的是string而不是stringbuilder
   sb.insert(0,"startstring");
   sb.delete(fromIndex,toIndex);
   sb.deleteCharAt(index);
   sb.replace(startIndex,endIndex,newString);
   sb.reverse();
   sb.toString();
   ```

4. equals()方法两个object对比会对比是否指向同一个内存空间（和相同）。但是String重写了equals方法让它评估字符串内容是否一致

5. 一般来说string会复用字符串池中的字符串，但是如果new String()的话就会避开池新建一个字符串

6. myString.intern() 方法会强制检查字符串池，如果找到相同的可能会导致两个string == 返回true。这只是为了考试，实际上intern和都不是一个好的string使用

7. array是一个object所以他也可以调用equals等方法，使用Array.toString(myArray)可以展示数组内容。array没有length方法但有length属性

8. Arrays.sort()可以进行排序，数字在字母前面，大写在小写前面

9. 对于已经排序好的可以使用Arrays.binarySearch()进行查找，找到了返回该index，如果没找到返回负数index，此index对应该值应该出现的位置

10. Arrays.compare() 如果第一个数组更小返回负数相等返回0更大返回正数。比较的必须是相同类型的数组 否则报错

11. Arrays.mismatch()如果相同返回-1否则返回第一个不同的index

12. Math.min() 返回最小值 Math.max() 返回最大值 Math.round() 返回四舍五入 Math.ceil() 返回入 Math.floor() 返回舍 Math.pow() 返回几次方 Math.random() 返回0-1的随机

13. Date & Time (以前都用的java.util.Date 但是现在基本使用java.time.\*)
    ```java
    LocalDate date = LocalDate.of(2023, 10, 5); // 2023-10-05
    LocalTime time = LocalTime.of(14, 30); // 14:30:00
    LocalDateTime dateTime = LocalDateTime.now(); // 当前日期时间
    ZonedDateTime zonedDt = ZonedDateTime.now(ZoneId.of("Asia/Shanghai"));
    //注意例子中全都是工厂模式，如果碰到 new LocalDateTime() 肯定要编译出错
    public static ZonedDateTime of(int year, int month, int dayOfMonth, int hour, int minute, int second, int nanos, ZoneId zone);
    public static ZonedDateTime of(LocalDate date, LocalTime time, ZoneId zone);
    public static ZonedDateTime of(LocalDateTime dateTime, ZoneId zone);
    
    DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");
    String formatted = dateTime.format(formatter); // 转为字符串
    LocalDateTime parsed = LocalDateTime.parse("2023-10-05 14:30", formatter); // 解析
    
    LocalDate tomorrow = date.plusDays(1); // 加1天
    LocalTime earlier = time.minusHours(2); // 减2小时
    LocalDateTime adjusted = dateTime.withMonth(12); // 调整月份为12月
    
    ZoneId zone = ZoneId.of("America/New_York");
    ZonedDateTime nyTime = ZonedDateTime.now(zone); // 纽约当前时间
    Instant instant = Instant.now(); // 当前时间戳（UTC
    
    Period.ofYears();
    Period.ofMonths();
    Period.ofWeeks();
    Period.ofDays();
    Period.of(year,month,day);
    Period period = Period.between(LocalDate.of(2023, 1, 1), LocalDate.now());//Duration 方法
    
    Duration.ofDays();
    Duration.ofHours();
    Duration.ofMinutes();
    Duration.ofSeconds();
    Duration duration = Duration.between(startTime, endTime);
    ```

14. String 是有 lines() 方法的，他用来通过分隔符 返回 Stream&lt;String

15. System.out.print(2+"") 包含了自动装箱，因为这里的运算先是int→Integer 然后调用toString。

16. String StringBuffer StringBuilder 都是final的，包装类也都是final的，System也是final的

17. if(a=b) 如果a,b是bool那么这个if是合法的，因为赋值语句是有返回值的，返回值就是赋值的值，这里是bool

18. trim方法属于String而不是StringBuilder

19. 为了构建一个Locale 必须的参数是language，第二参数是可选的，也就是country

20. Instant 和 Duration 是线程安全的, java.util里的Date不是线程安全的，java.time里面的几乎全都是线程安全的因为他们不可变

21. 时间这一块儿用的基本都是java.time里的，使用的format也是 java.time.format.\*

22. LocalDate LocalTime 之类的都实现了TemporalAccessor 而跟java.util 没有任何关系

23. getBundle ("message", my Locale("ch","CH")) or getBundle("message",Locale.CHINA);

24. String 没有append方法（只有StringBuffer StingBuilder有），但是有concat方法

# 第五章 Methods

## 5.1 Content and notes

1. 局部变量除了final以外不可以使用任何修饰符。final修饰之后普通变量不能再更改值，object不能改引用，数组不能设置为null
2. Effective final 指虽然没有被final修饰但是一旦赋值之后就不会修改的变量，但是要注意这是编译期决定的，所以编译期潜在修改情况也会被认为可能被修改
3. 可变参数最多只能有一个并且只能放在方法的最末尾，可以传数组也可以传很多值
4. 单一可变参数的调用可以不传值，但是不可以传null 因为不传值是编译器为你添加的空数组，获取的时候用数组的形式接收就行
5. Static修饰的变量是可以通过实例去修改的，一旦某个实例修改了这个值所有的实例共享的值也就都发生变化了，这就是为什么我们需要把常量定义为private static final 而不是单纯的static了
6. 静态方法可以调用静态方法，但是不能调用没有实例指向的实例方法，实例方法可以调用实例方法和静态方法
7. static变量可以不赋值，可以在类的任何地方赋值也可以通过实例赋值
8. static final变量需要声明的时候赋值或者static块赋值否则报错。final单独修饰的时候在构造器执行之前赋值就行，否则会报错
9. {}中的代码是实例创建的时候执行且每次实例化都执行，static {} 的代码是类加载的时候执行而且只执行一次
10. static import 是用来引入静态方法或者静态变量的，引入之后可以直接使用，不需要类前缀或者实例前缀
11. Autoboxing 和 unboxing 是不需要显示的转换的 比如 Integer i = 5; int num = i; 都是自动装箱和拆箱。但是 Long longValue = 8;会失败，因为自动装箱拆箱只有一步，不会自动类型转换之后装箱这种复合功能
12. 方法重载永远会优先调用更加细化的类型，比如String 和Object参数类型的方法，传入字符串的时候永远是String优先于Object

# 第六章 Class Design

## 6.1 Content and notes

1. 继承时public 和protected 变量都会继承下去，package只有在同一个包的时候会被继承，private的不会被继承（子类拥有该成员空间但是无法访问）
2. 类不能重写父类的构造方法
3. 如果构造方法通过this调用其他构造那么必须在初注释的第一行出现，如果通过super调用其他构造那么也必须出现在除注释的第一行
4. 如果构造方法没有this或super那么会默认调用无参数super()，如果你的父类没有无参数构造器 那么子类的构造函数里必须在第一行显示的调用一个this或者super，this一直指向到一个super上，不然会造成循环同样无法编译
5. 一个小trick，如果父类定义了一个有参构造，那么子类必须定义自己的构造函数，否则会因为默认调用父类无参构造失败而报错
6. 类的初始化：先初始化父类再初始化子类，先处理所有的静态变量的声明然后处理静态代码块都按照从上倒下的顺序。在执行main方法之前就要先初始化各个类所以打印结果不会按照main方法的顺序来，要先把类初始化了再从main顺序开始。只有非final变量才可以不赋值，final不管如何都要在某个位置赋值，要么是声明的地方，要么是静态代码块要么是实例代码块要么是构造方法（注意final和static final的区分）。总之真实使用之前是必须有值的
7. static final需要在声明或静态块赋值一次，non-static final需要在声明或代码块或者构造器赋值一次，非final的参数如果不赋值会被赋值一个默认值
8. 实例的初始化：初始化父类的类成员->初始化子类的类成员->处理父类的成员变量和代码块->处理子类所有成员变量和代码块->按顺序调用构造函数  
9. 方法重写会往更加细化且使用更加开放的方向演变：
   1. 子类的访问修饰符不能更加严格（父类可以使用这个方法，子类竟然没权限使用，子类应该是加强版的父类才对）
   2. 不能抛出更新更宽的异常（我按照父类声明的异常进行处理，子类怎么能抛一个我不知道的异常出来呢）
   3. 返回类型可以是父类返回类型或其子类（我按照父类逻辑写的，所以至少返回父类或他的子类才对）  
10. 覆盖，父类不可见的方法子类同样可以声明一个一模一样的去覆盖。但是如果方法在父类中是static那么子类也必须是static的。 重写一个实例方法之后除了使用super否则永远使用对象类型的方法，但是覆盖一个变量或者静态方法之后具体使用哪个变量取决于你的引用是什么引用
11. 抽象方法只能是实例方法，不能是变量构造器或者静态方法。抽象方法只能在抽象类中，非抽象类要继承抽象类就必须实现所有抽象方法
12. 抽象类尽管不能被直接实例化，但是它仍然可以定义自己的构造方法，抽象类的构造方法唯一只在子类实例化的时候调用
13. 一个抽象类实现了interface之后并不是一定要实现所有的方法
14. abstract 和final 不能同时使用，abstract 也不能和static一起使用
15. 不可变类： 
    1. 类要标记为final或者让所有构造方法都private 
    2. 实例变量都是private final的 
    3. 没有任何setter方法
    4. 类中没有任何引用变量可被修改
    5. 使用一个构造器设置所有字段

## 6.2 Topics

### 6.2.1 隐藏

父类定义了public 成员变量/成员方法/静态变量/静态方法 的时候通过多态调用会发生什么： Parent p = new Child();

1. 成员变量：p.number 输出 parent的number（成员变量不参与多态，由引用类型决定）
2. 成员方法：p.getNumber 调用child的方法（成员方法参与多态，运行时对象类型决定）
3. 静态变量：p.staticNumber 输出parent 的number（静态变量不参与多态，由引用类型决定）
4. 静态方法：p.staticMethod 调用parent 的方法（静态变量不参与多态，由引用类型决定）

总结：

**重写（override）方法**：在子类中重写父类的实例方法后：

- **无论引用变量是父类还是子类类型**，运行时都会调用子类的那个方法。
- 唯一的例外是 `super.method()`，它显式调用父类的方法。

**隐藏（hiding）方法或变量**：发生在下面两种情况：

- **静态方法被隐藏**（子类定义了与父类同名的 `static` 方法）
- **变量被隐藏**（子类定义了与父类同名的变量）

对于这两种隐藏：

- 如果你用的是 **父类引用类型**，访问的是**父类的版本**
- 如果你用的是 **子类引用类型**，访问的是**子类的版本**

# 第七章 Beyond Classes

## 7.1 Content and notes

1. 接口有很多隐含修饰符，接口的抽象方法如果没有private修饰就默认添加public abstract，接口变量若无修饰默认添加public static final

2. 接口不能被final修饰，因为实现其实也是继承的一种

3. 接口可以继承很多个接口，该接口的实现类必须实现这个接口继承过来的所有接口的所有抽象方法

4. 总的来说，接口只能继承接口（不能继承类或者实现接口）。类可以实现接口和继承类（不能继承接口或者实现类）

5. 接口现在支持private方法以供内部方法调用，但是不支持private变量，也不支持protected 和 package方法和变量

6. 接口的default方法是为了向下兼容，为所有实现类提供一个公共方法，实现类可以决定是使用默认方法还是重写自己的方法

7. default方法是隐含public的不能被修饰为abstract/final/static。如果一个类继承了多个同一签名的默认方法就必须重写属于自己的这个方法

8. 接口的静态方法必须有方法体，不能被标记为abstract/final。静态方法不能被实现类继承，调用只能通过 InterfaceA.staticMethod() 的方法调用 

9. 枚举类不可以被继承。枚举可以实现接口，只要实现相关的接口方法就行

10. 复杂枚举是可以包含构造器，方法和字段的，但是很少这么做，而且构造器都是隐含private的，你没办法继承或者实例化它

11. 调用私有方法的时候通过Season.SUMMER.myMethod()去调用。如果枚举定义了抽象方法，那么每一项都必须实现这个方法
    ```java
    //例子：
    for(var season: Season.values()) {
    	System.out.println(season.name() + " " + season.ordinal());
    }
    Season s = Season.valueOf("SUMMER"); // SUMMER必须完全匹配包括大小写
    Season summer = Season.SUMMER;
    var message = switch(summer){ 
        case Season.WINTER -> "Get out the sled!"; // 编译失败，多余的声明
    }
    ```

12. sealed 类用来限制可以继承他的类达到更好的封装，关键字sealed non-sealed表示是否封装，关键字permits表示允许继承该类的类列表

13. 注意sealed类和permits的类必须在同一个包内方便编译器管理，否则会编译出错

14. 继承自sealed类必须声明自己为final/sealed/non-sealed 来明确表示自己还是否要密封，permits列表里的类必须去继承它否则就报错

15. 如果同一个文件里public类是sealed类，如果不写permits默认只有当前文件其他类才可以被permits，如果不在同一个文件内省略permits会报错

16. 接口也是可以被sealed修饰进行密封的，唯一的不同是这里的sealed指的是继承和实现两种，也就是说permits列表里的可以继承也可以实现他

17. record类通过类名后面括号里的内容决定当前record的包含的成员变量，字段访问方法，equals方法，hashCode方法，toString方法

18. record 类自动添加一个全参数的构造函数，但是要注意构造函数按照定义的字段顺序来的，如果弄反了会编译失败

19. record的字段都是默认final的没有setter方法，record类也是final的无法被继承

20. record同样可以实现接口或者密封接口，只要能把所有抽象方法都实现了就行。

21. record如果你要定义自己的构造函数必须所有参数都赋值，你也可以定义紧凑构造函数，他会在默认赋值所有字段的基础上，再使用你定义的规则如下

22. record会自动生成标准构造，但是你自己定义了紧凑或标准构造之后它就不生成了，你的紧凑构造会自动为所有值赋值，你需要做的是处理验证逻辑

23. record可以重载构造函数，但是在第一行必须显式的用this调用其他构造，如果没有写其他构造就必须显示调用默认标准构造

24. record自己定义的紧凑构造和标准构造只能有一个，紧凑构造函数没有小括号。注意如果在紧凑构造里面使用this去赋值是会失败的因为无法修改final
    ```java
    public record Crane(int numberEggs, String name) { }
    public record Crane(int numberEggs, String name) {
    	public Crane {
    		if (numberEggs < 0) throw new IllegalArgumentException();
    		name = name.toUpperCase(); // 报错
    	}
    }
    ```

25. 嵌套类1-内部类inner class：在类的成员级别定义的非静态类型，非静态的内部类，和类的方法，变量等平级，可以被声明为任意访问修饰符，可以继承类或实现接口，可以被标注为abstract or final，可以访问包括private在内的所有外部类成员

26. 嵌套类2-静态嵌套类static nested class：在类的成员级别定义的静态类型，静态嵌套类就是用static修饰的内部类，它不用经过外部类就可以实例化（也就是可以直接new Pet() 而不需要 new person().new Pet()，但是坏处是无法访问外部类的实例变量了。但是外部类可以很简单的访问静态内部类的方法和字段比如new Pet().bark()

27. 静态内部类和非静态内部类的区别：如果内部类需要访问外部类的时候用非静态内部类，内部类不需要访问外部类的似乎后用静态内部类。静态内部类像是更加独立一下但是不被其他类所需要。在构建pojo的时候会用到。非静态内部类更像是在普通成员方法太过复杂的时候寻求的另一种解决方案，比如Document类的非静态内部类Editor，这个内部类肯定需要获得Document的内从才能工作。他像是一个可以操作的成员方法，但是更高级更可读。非静态内部类要通过两个对象才能访问，外部类先新建一个外部对象去调用外部方法，外部方法会创建一个内部类对象去访问内部方法。先new Document 然后调用document的updateMyDoc()方法，这个方法会new Editor。然后调用editor.updateTheDoc()。 也可以new Document().new Editor().updateTheDoc();这样通过两个object完成内部类方法的调用

28. 嵌套类3-局部类local class：在方法体内定义的类，局部类就是在方法体里定义的类，它没有访问修饰符，可以被定义为final or abstract 它们可以访问外部类的所有字段和方法，可以访问final 和effectively final的当前方法的局部变量（非这两种场景的话就会报错）。  

29. 嵌套类4-匿名类anonymous class：没有名称的局部类的一种特殊情况，匿名内部类就是局部类的无名版，但是他必须要继承类或者实现接口，否则都无法知道它究竟是个什么类型。由于他的代码都是一行搞定算是一条语句所以一定要注意分号是否漏掉了。匿名类无法同时继承一个类和实现一个接口。  

30. 多态：一般来说我们的引用有三种情况：1.和对象是同一个类型2.是这个对象的的父类型3.是这个对象实现的一个接口类型。一般来说一个object的类型决定了它所具有的属性。而这个object的引用决定了它有那些可用的方法和字段。换句话说：object的类型决定自己有什么，reference的类型决定了什么是可以用的。在进行类型转换的时候，下级像上级转不需要显示的转换，上级像下级转换需要明确的语法进行转换，运行时的转换可能拋错ClassCastException。编译期Java禁止不相关的类进行转换。但是在interface中就不一样了，MyInterfaceA m = (MyInterfaceA) instanceOfSubOfMyInterfaceB;对于interface的使用比较灵活，所以编译器就算察觉到了类型转换不合法但是也不会提示你，因为他担心你的一个子类会去实现这个接口，不过实际运行中会报错  

31. instanceof运算符在使用时，如果两个类没有任何关系那么编译不会通过，如果两个类有继承关系就会通过，大不了在运行的时候碰到不对的类型报异常 。在转型的时候可以提一嘴，向上转型永远都能成功，向下转型必须要你真的是那个类型才能转，比如 Dog dog = (Dog) animal 要想运行的时候不报错必须animal是Dog类型而不是Cat类型，也就是说定义的时候是 Animal animal = new Dog(); 才可以。  

32. 在继承关系中如果子类重写了父类的成员方法那么任何对该方法的调用都会被替换为子类的。就算是子类调用父类方法，父类方法在调用这个被重写的方法时也会找到子类去。如果这不是你的初衷那么定义该方法为final就不允许重写这个方法了。但是静态方法是另一种情况，成员方法可以重写，静态方法只能覆盖。因为静态方法是静态绑定的，成员方法是动态绑定的，也就是说成员方法具体要调用哪一个要通过运行时的类型去判断，但是静态方法要怎么调在加载类的时候就绑定好了

## 7.2 Topics

### 7.2.2 Records

| **特性**                | **说明**                                  |
| ----------------------- | ----------------------------------------- |
| final 类                | 不能被继承（隐式是 final）                |
| 字段默认 private final  | 不可变                                    |
| 自动生成构造方法        | Canonical constructor                     |
| 自动生成 getters        | 方法名就是字段名（**不是** getName()）    |
| 不能定义实例字段        | 除了定义时的 component 字段，不能新增字段 |
| 可添加静态字段或方法    | ✅ 支持                                    |
| 可实现接口              | ✅ 支持                                    |
| 可定义静态嵌套类        | ✅ 支持                                    |
| 不能继承类              | ❌                                         |
| 不能显式 extends 其他类 | ❌（除了 java.lang.Record）                |

1. record是不可变的，隐含修饰final，不能继承或者被继承但是可以实现接口，字段构造器全自动，没有get方法可以通过字段名直接获取
2. record 只能使用声明的时候定义的实例变量，不能添加
3. record 只会默认添加标准的全参构造器，需要其他构造器就要你自己去定义了，定义的时候第一行必须引用其他构造器。record即使你提供了一些构造器，如果没提供标准的，编译器仍然会提供一个标准的。如果你想提供标准的，你需要提供简易版的，然后编译器会默认为你添加相关字段，你只需要编写你想要的额外逻辑就行
4. record 可以定义实例方法，可以定义静态字段，静态方法，静态嵌套类。唯独不能额外定义实例字段
5. record可以定义在最顶层，可以定义在一个类的内部作为嵌套类，也可以定义在方法内部作为局部内部类
6. record作为嵌套类是隐式静态的，你可以在成员嵌套类的时候用冗余的static修饰，但是不允许在局部内部类的时候用static修饰
7. record不能被修饰为abstract sealed non-sealed

### 7.2.3 Sealed Class

1. 每个继承者必须在 permits 列表中声明，每个继承者必须是 final**、**sealed 或 non-sealed的，继承者必须与sealed 类在同一个模块或包中
2. 抽象类和接口都可以用 sealed 修饰。sealed接口的实现类也需要 permits + 三选一，如果继承者是record就不需要额外的修饰符因为它已经是final的了
3. 定义了sealed类就意味着它一定可以被继承，你不写permits 或者permits后面什么都不跟 是编译错误的写法
4. sealed类可以实现接口但是要注意定义顺序， sealed MyClass inplements OtherClass permits MyClass1 如果permits在前面implements在后面就会编译报错
5. sealed类是可以被定义为abstract的

# 第八章 Lambdas and Functional Interfaces

## 8.1 Content and notes

1. lambda的语法就像：(Animal a) -> { return a.canHop(); } 或者简略成：a -> a.canHop() 

2. 上题 如果入参只有一个那么就可以像这样省略括号和类型。如果方法体只有一行就可以像这样省略括号和return

3. 如果方法体没省略要记得检查每一行的分号。注意lambda的传参，如果有一个参数使用了var那么所有入参都使用var，如果不一致会报错 

4. 函数式接口指只有一个抽象方法的接口（注意“一个”和“抽象方法”），但是接口从object继承过来的方法如果被重写为抽象方法是不算做这里的计数的

5. 如果一个lambda只传一个参数可以简略为类似：InterfaceA a = System.out::println; 这相当于 InterfaceA a = s->System.out.println(s);  

6. 方法引用的四种形式：

   1. 调用静态方法Math::round; 
   2. 调用特定对象的实例方法比如：str::startWith; 
   3. 通过参数调用实例方法比如String::startWith; 
   4. 调用构造比如String::new;  

7. 所有的方法引用都可以转成lambda但是不是所有lambda都可以转成方法的引用

8. 转换的时候注意是方法的引用而不是方法的执行。比如str::startsWith("Zoo"); 会报错因为它想要的是方法的执行  

9. 内建函数式接口（注意方法名/返回类型等）比如：consumer返回void， predicable返回boolean，其他的都返回范型类型

   | **Functional interface**  | **Return type** | **Method name** | **Scenario**                       | **Convenience methods**                                      |
   | ------------------------- | --------------- | --------------- | ---------------------------------- | ------------------------------------------------------------ |
   | Supplier&lt;T&gt;         | T               | get()           | 不需要输入就生成输出T              |                                                              |
   | Consumer&lt;T&gt;         | void            | accept(T)       | 接受输入T但是不返回任何东西        | andThen() 比如 lambda1.andThen(lambda2)                      |
   | BiConsumer&lt;T, U&gt;    | void            | accept(T,U)     | 接受输入T, U但是不返回任何东西     |                                                              |
   | Predicate&lt;T&gt;        | boolean         | test(T)         | 接收输入T用来过滤或者判断          | and() negate() or()  比如 lambda1.and(lambda2) 但是negate是求反 |
   | BiPredicate&lt;T, U&gt;   | boolean         | test(T,U)       | 接收输入T, U用来过滤或者判断       |                                                              |
   | Function&lt;T, R&gt;      | R               | apply(T)        | 接收输入T，返回输出R               | andThen() compose()                                          |
   | BiFunction&lt;T, U, R&gt; | R               | apply(T,U)      | 接收输入T, U 返回输出R             |                                                              |
   | UnaryOperator&lt;T&gt;    | T               | apply(T)        | 和Function类似，但是输入输出同类型 |                                                              |
   | BinaryOperator&lt;T&gt;   | T               | apply(T,T)      | 和Function类似，但是输入输出同类型 |                                                              |

10. 基本数据类型的函数式接口

    | **接口名**              | **返回类型** | **抽象方法名**  | **参数个数（类型）** | **用途说明**                 |
    | ----------------------- | ------------ | --------------- | -------------------- | ---------------------------- |
    | DoubleSupplier          | double       | getAsDouble()   | 0                    | 提供一个 double 类型的值     |
    | IntSupplier             | int          | getAsInt()      | 0                    | 提供一个 int 类型的值        |
    | LongSupplier            | long         | getAsLong()     | 0                    | 提供一个 long 类型的值       |
    | DoubleConsumer          | void         | accept(double)  | 1 (double)           | 消费一个 double 类型的值     |
    | IntConsumer             | void         | accept(int)     | 1 (int)              | 消费一个 int 类型的值        |
    | LongConsumer            | void         | accept(long)    | 1 (long)             | 消费一个 long 类型的值       |
    | DoublePredicate         | boolean      | test(double)    | 1 (double)           | 对 double 值进行条件判断     |
    | IntPredicate            | boolean      | test(int)       | 1 (int)              | 对 int 值进行条件判断        |
    | LongPredicate           | boolean      | test(long)      | 1 (long)             | 对 long 值进行条件判断       |
    | DoubleFunction&lt;R&gt; | R            | apply(double)   | 1 (double)           | 接收 double，返回任意类型 R  |
    | IntFunction&lt;R&gt;    | R            | apply(int)      | 1 (int)              | 接收 int，返回任意类型 R     |
    | LongFunction&lt;R&gt;   | R            | apply(long)     | 1 (long)             | 接收 long，返回任意类型 R    |
    | DoubleUnaryOperator     | double       | applyAsDouble() | 1 (double)           | 接收 double，返回 double     |
    | IntUnaryOperator        | int          | applyAsInt()    | 1 (int)              | 接收 int，返回 int           |
    | LongUnaryOperator       | long         | applyAsLong()   | 1 (long)             | 接收 long，返回 long         |
    | DoubleBinaryOperator    | double       | applyAsDouble() | 2 (double, double)   | 接收两个 double，返回 double |
    | IntBinaryOperator       | int          | applyAsInt()    | 2 (int, int)         | 接收两个 int，返回 int       |
    | LongBinaryOperator      | long         | applyAsLong()   | 2 (long, long)       | 接收两个 long，返回 long     |

11. 基本数据类型的转换函数式接口

    | **接口名**                     | **返回类型** | **抽象方法名**  | **参数个数（类型）** | **用途说明（简要）**           |
    | ------------------------------ | ------------ | --------------- | -------------------- | ------------------------------ |
    | ToDoubleFunction&lt;T&gt;      | double       | applyAsDouble() | 1 (T)                | 从对象 T 映射为 double         |
    | ToIntFunction&lt;T&gt;         | int          | applyAsInt()    | 1 (T)                | 从对象 T 映射为 int            |
    | ToLongFunction&lt;T&gt;        | long         | applyAsLong()   | 1 (T)                | 从对象 T 映射为 long           |
    | ToDoubleBiFunction&lt;T, U&gt; | double       | applyAsDouble() | 2 (T, U)             | 从 T 和 U 映射为 double        |
    | ToIntBiFunction&lt;T, U&gt;    | int          | applyAsInt()    | 2 (T, U)             | 从 T 和 U 映射为 int           |
    | ToLongBiFunction&lt;T, U&gt;   | long         | applyAsLong()   | 2 (T, U)             | 从 T 和 U 映射为 long          |
    | DoubleToIntFunction            | int          | applyAsInt()    | 1 (double)           | 从 double 映射为 int           |
    | DoubleToLongFunction           | long         | applyAsLong()   | 1 (double)           | 从 double 映射为 long          |
    | IntToDoubleFunction            | double       | applyAsDouble() | 1 (int)              | 从 int 映射为 double           |
    | IntToLongFunction              | long         | applyAsLong()   | 1 (int)              | 从 int 映射为 long             |
    | LongToDoubleFunction           | double       | applyAsDouble() | 1 (long)             | 从 long 映射为 double          |
    | LongToIntFunction              | int          | applyAsInt()    | 1 (long)             | 从 long 映射为 int             |
    | ObjDoubleConsumer&lt;T&gt;     | void         | accept()        | 2 (T, double)        | 对象 + double 参数的消费型接口 |
    | ObjIntConsumer&lt;T&gt;        | void         | accept()        | 2 (T, int)           | 对象 + int 参数的消费型接口    |
    | ObjLongConsumer&lt;T&gt;       | void         | accept()        | 2 (T, long)          | 对象 + long 参数的消费型接口   |


# 第九章 Collections and Generics

## 9.1 Content and notes

1. 集合框架常见的包括List Set Queue Map。虽然Map没有继承Collection但是如果题目说到 Java collections framwork的时候其实是包括了Map的。  

2. List常见的两类：ArrayList像一个自动伸缩的数组检索性能更强，而LinkedList继承了List和Deque，更新和删除性能更强

3. 创建List的三种工厂方法：

   ```java
   Arrays.asList(varargs);
   List.of(varargs);
   List.copyOf(collection)
   ```

4. 创建List的构造方法
   ```java
   var list1 = new ArrayList<String>();
   var list2 = new ArrayList<String>(list1);
   var list3 = new ArrayList<String>(10);
   ```

5. List方法：
   ```java
   public boolean add(E element);
   public void add(int index, E element);
   public E get(int index);
   public E remove(int index);
   public default void replaceAll(UnaryOperator<E> op);
   public E set(int index, E e);
   public default void sort(Comparator<? super E> c);
   list.toArray() //list转成array
   ```

6. Hashset是用hash值来快速定位到其对象成员的，成员的hash值并不唯一，如果重复了就证明发生了hash碰撞

7. TreeSet是按照树形结构排序的，但缺点是插入删除花费的时间更长

8. Set的工厂方法： Set.of() Set.copyOf()

9. Queue 和 Deque:
   ```java
   //Queue方法
   public boolean add(E e); //这两个方法从尾部增加数据，第一个会抛异常
   public boolean offer(E e);
   public E element (); //这两个方法从头部读数据，第一个会抛异常
   public E peek();
   public E remove(); //这两个方法从头部移除数据，第一个会抛异常
   public E poll();
   //Deque 方法
   public void addFirst(E e);//从头部增加数据
   public boolean offerFirst(E e); 
   public void addLast(E e); //从尾部增加数据
   public boolean offerLast(E e);
   public E getFirst(); //从头部读取数据
   public E peekFirst();
   public E getLast(); //从尾部读取数据
   public E peekLast();
   public E removeFirst(); //从头部移除数据
   public E pollFirst();
   public E removeLast(); //从尾部移除数据
   public E pollLast();//用双端队列实现栈的栈方法
   public void push(E e); //添加数据
   public E pop(); //移除数据
   public E peek(); //查看数据
   ```

10. Map的工厂方法
    ```java
    Map.of("key1", "value1", "key2", "value2");
    Map.ofEntries();
    Map.entry("key1", "value1"),Map.entry("key2", "value2"));
    ```

11. HashMap 会让检索更加快捷，但是会损失顺序，TreeMap是有序的但是随着树变大添加和检索的时候会更加耗时

12. Map方法： 注：contains是Collection的方法，Map没有这个方法，但是有containsKey containsValue

    ```java
    public void clear();
    public boolean containsKey(Object key);
    public boolean containsValue(Object value);
    public Set<Map.Entry<K,V>> entrySet(); //返回k-v的set
    public void forEach(BiConsumer<K key, V value>);
    public V get(Object key);
    public V getOrDefault(Object key, V defaultValue);
    public boolean isEmpty();
    public Set<K> keySet();
    public V merge(K key, V value, Function(<V, V, V> func));//如果key不存在则添加KV，如果存在将对应的v改成调用方法生成的V
    public V put(K key, V value);
    public V putIfAbsent(K key, V value);
    public V remove(Object key);
    public V replace(K key, V value);
    public void replaceAll(BiFunction<K, V, V> func); //将所有key对应的value利用function从新计算然后更新
    public int size();
    public Collection<V> values()
    ```

    | **Type**  | **Can contain duplicate elements?** | **Elements always ordered?** | **Has keys and values?** | **Must add/remove in specific order?** |
    | --------- | ----------------------------------- | ---------------------------- | ------------------------ | -------------------------------------- |
    | **List**  | Yes                                 | Yes (by index)               | No                       | No                                     |
    | **Map**   | Yes (for values)                    | No                           | Yes                      | No                                     |
    | **Queue** | Yes                                 | Yes (retrieved in order)     | No                       | Yes                                    |
    | **Set**   | No                                  | No                           | No                       | No                                     |

    | **Type**       | **Java Collections Framework Interface** | **Sorted?** | **Calls** hashCode()**?** | **Calls** compareTo()**?** |
    | -------------- | ---------------------------------------- | ----------- | ------------------------- | -------------------------- |
    | **ArrayDeque** | Deque                                    | No          | No                        | No                         |
    | **ArrayList**  | List                                     | No          | No                        | No                         |
    | **HashMap**    | Map                                      | No          | Yes                       | No                         |
    | **HashSet**    | Set                                      | No          | Yes                       | No                         |
    | **LinkedList** | List, Deque                              | No          | No                        | No                         |
    | **TreeMap**    | Map                                      | Yes         | No                        | Yes                        |
    | **TreeSet**    | Set                                      | Yes         | No                        | Yes                        |

13. 数据排序（记住数字在字母前面，大写在小写前面）  
    ```java
    //Comparable Comparator:
    public interface Comparable<T> {
    	int compareTo(T o);
    }
    //一个类实现了这个接口和compareTo方法之后就可以 Collections.sort(ducks);会自动调用compareTo方法进行排序了
    //String的compareTo规则：相同返回0前者更小返回负数前者更大返回正数。记住compareTo里 id-a.id 是升序
    //注意写法的一致性问题： 如果compareTo 的结果是0 那么equals（）方法的调用也必须是二者相同。compareTo不为0那么equals不相同
    
    Comparator<Duck> byWeight = new Comparator<Duck>() {
    	public int compare(Duck d1, Duck d2) {
    		return d1.getWeight()-d2.getWeight();
        }
    };
    Collections.sort(ducks, byWeight);
    ```

    | **Difference**                                             | **Comparable** | **Comparator** |
    | ---------------------------------------------------------- | -------------- | -------------- |
    | **Package name**                                           | java.lang      | java.util      |
    | **Interface must be implemented by class being compared?** | Yes            | No             |
    | **Method name in interface**                               | compareTo()    | compare()      |
    | **Number of parameters**                                   | 1              | 2              |
    | **Common to declare using a lambda**                       | No             | Yes            |

    | **Method**                | **Description**                                              |
    | ------------------------- | ------------------------------------------------------------ |
    | comparing(function)       | Compare by results of function that returns any Object (or primitive autoboxed). |
    | comparingDouble(function) | Compare by results of function that returns double.          |
    | comparingInt(function)    | Compare by results of function that returns int.             |
    | comparingLong(function)   | Compare by results of function that returns long.            |
    | naturalOrder()            | Sort using the order specified by the Comparable implementation on the object itself. |
    | reverseOrder()            | Sort using the **reverse** of the order specified by Comparable. |

14. 注意binarySearch需要一个正序的list如果是逆序或者无序就会返回-1.

15. 对List进行sort：bunnies.sort((b1, b2) -> b1.compareTo(b2)); 但是map和set没有排序方法因为他们不需要排序 。

16. 范型：范型是一个语法糖，编译器会替你检测类型是否匹配，但是编译期过了之后这个类型会变成object此时不具有任何类型意义。这就是类型擦除

17. 由于类型擦除，在定义重载方法的时候要如果重载的是范型类型会编译不通过。比如 List&lt;Object&gt; List&lt;String&gt; 这两个参数会导致该方法是同一个方法

18. 继承之后的范型方法返回值是协变的，子类方法返回值跟父类方法相比范型需要相同但大类型子类是父类的子对象： 父类返回List&lt;CharSqeuence&gt; 子类： ArrayList&lt;CharSequence&gt; 

19. 实现范型接口的三种选择：1.指定范型类型 2.创建范型类 3.不使用范型，范型都用object替换

20. 声明范型方法的时候要在返回值前面定义出来比如：public static &lt;T&gt; void prepare(T t) { ...}  

21. 声明一个范型record 的语法：public record CrateRecord&lt;T&gt;(T contents) { ...} 

22. List<String>不能传给List<Object> 因为Object是更宽泛的类，它实际上可能被赋一个更窄的值，这个时候和String就不一定适配了。但是List<String> 可以传给List<?> 因为?可以接受任意类型的参数

23. 范型是不具备协变的，也就是说 String是Object的子类，但是List<String> 不能直接赋值给 List<Object> 要使用通配符才行

24. 范型List<?> 和 List<T> 的区别：?没有定义类型，不会让你调用add方法，因为你不知道里面装的是什么类型，而T可以add因为他运行的时候就知道类型

25. List<? extends Animal> list 不能添加只能访问，如果你添加一个Sparrow对象，但是后来发现这是一个List<Bird> 那就肯定错误了，编译器不允许你这样做

26. 而List<? super String> list 允许你添加，因为无论它是一个List<String>或者List<Object> 添加一个string进去都是被允许的

27. 范型会导致使用的类型被当作object，为了让范型更加有用：

| **类型**               | **语法**       | **示例**                                                     | **备注**                           |
| ---------------------- | -------------- | ------------------------------------------------------------ | ---------------------------------- |
| **无界通配符**         | ?              | List&lt;?&gt; a = new ArrayList&lt;String&gt;();             | 元素会被当作object，能用的方法很少 |
| **上界通配符**（协变） | ? extends 类型 | List&lt;? extends Exception&gt; a = new ArrayList&lt;RuntimeException&gt;(); | 元素是该类型子类，适合读不适合写   |
| **下界通配符**（逆变） | ? super 类型   | List&lt;? super Exception&gt; a = new ArrayList&lt;Object&gt;(); | 元素是该类的超类，适合写不适合读   |

## 9.2 Topics

### 9.2.3 About collection

常见的所有集合几乎都不是线程安全的，使用线程安全的集合有三种方式

1. 使用普通集合的“同步包装器”

   ```java
   List<String> list = Collections.synchronizedList(new ArrayList<>());
   Map<String, String> map = Collections.synchronizedMap(new HashMap<>());
   ```

2. 使用java.util.concurrent 中的集合并发类

   | **类型**       | **类名**                                | **特点/适用场景**          |
   | -------------- | --------------------------------------- | -------------------------- |
   | List           | CopyOnWriteArrayList                    | 适用于读多写少（读写复制） |
   | Set            | CopyOnWriteArraySet                     | 基于 CopyOnWriteArrayList  |
   | Map            | ConcurrentHashMap                       | 细粒度锁，适合高并发读写   |
   | Queue          | ConcurrentLinkedQueue                   | 非阻塞队列（基于链表）     |
   | Deque          | ConcurrentLinkedDeque                   | 非阻塞双端队列             |
   | Blocking Queue | LinkedBlockingQueue, ArrayBlockingQueue | 线程间通信，支持阻塞操作   |

3. 使用线程安全的集合比如：Vector（线程安全版的 ArrayList）Hashtable（线程安全版的 HashMap）

   | **知识点**                                 | **是否常考** | **备注**                        |
   | ------------------------------------------ | ------------ | ------------------------------- |
   | 哪些集合默认线程安全？                     | ✅            | Vector/Hashtable ✅，ArrayList ❌ |
   | Collections.synchronizedXXX() 使用方式     | ✅            | 包装 + 手动同步迭代             |
   | 并发集合类（CopyOnWriteXXX/ConcurrentXXX） | ✅            | 场景题、特性题、性能题          |
   | 哪些集合适合读多写少？                     | ✅            | CopyOnWriteArrayList            |
   | 哪些集合适合高频并发读写？                 | ✅            | ConcurrentHashMap               |
   | synchronizedList 是否迭代线程安全？        | ✅            | ❌，除非手动同步                 |
   | HashMap 线程安全？                         | ✅            | ❌                               |


# 第十章 Streams

## 10.1 Content and notes

1. Ｏptional: 常见实例方法

   | **Method**              | **When Optional is empty**                       | **When Optional contains value** |
   | ----------------------- | ------------------------------------------------ | -------------------------------- |
   | get()                   | Throws NoSuchElementException                    | Returns the value                |
   | ifPresent(Consumer c)   | Does nothing                                     | Calls Consumer with the value    |
   | isPresent()             | Returns false                                    | Returns true                     |
   | orElse(T other)         | Returns the other parameter                      | Returns the value                |
   | orElseGet(Supplier s)   | Calls and returns result from Supplier           | Returns the value                |
   | orElseThrow()           | Throws NoSuchElementException                    | Returns the value                |
   | orElseThrow(Supplier e) | Throws exception created by calling the Supplier | Returns the value                |

2. 流基础：
   ```java
   //创建有限流
   Stream<String> empty = Stream.empty(); 
   Stream<Integer> fromArray = Stream.of(1, 2, 3); 
   //创建无限流
   Stream<Double> randoms = Stream.generate(Math::random);
   Stream<Integer> oddNumbers = Stream.iterate(1, n ‑> n + 2);
   //受控的有限流
   Stream<Integer> oddNumberUnder100 = Stream.iterate(
       1, // seed
       n -> n < 100, // Predicate to specify when done
       n -> n + 2
   ); // UnaryOperator to get next value
   ```

3. 创建流的常见方法：

   | **Method**                                     | **Finite or Infinite?** | **Notes**                                                    |
   | ---------------------------------------------- | ----------------------- | ------------------------------------------------------------ |
   | Stream.empty()                                 | Finite                  | 创建一个空的 Stream（无元素）。                              |
   | Stream.of(varargs)                             | Finite                  | 创建包含给定元素的 Stream。                                  |
   | collection.stream()                            | Finite                  | 从一个 Collection 创建顺序 Stream。                          |
   | collection.parallelStream()                    | Finite                  | 从 Collection 创建可并行执行的 Stream。                      |
   | Stream.generate(Supplier)                      | Infinite                | 通过反复调用 Supplier 来生成无限 Stream（惰性求值）。        |
   | Stream.iterate(seed, UnaryOperator)            | Infinite                | 从种子值开始，依次对每个元素应用 UnaryOperator 来生成下一个值（无限）。 |
   | Stream.iterate(seed, Predicate, UnaryOperator) | Finite or Infinite      | 类似上面方式，但会在 Predicate 返回 false 时终止（可有限）。 |

4. 常见的流终止操作：

   | **Method**                            | **Infinite Stream 会终止吗？** | **Return Value**             | **是否是归约操作（Reduction）** |
   | ------------------------------------- | ------------------------------ | ---------------------------- | ------------------------------- |
   | count()                               | ❌ 不会终止                     | long                         | ✅ 是                            |
   | min() / max()                         | ❌ 不会终止                     | Optional&lt;T&gt;            | ✅ 是                            |
   | findAny() / findFirst()               | ✅ 会终止                       | Optional&lt;T&gt;            | ❌ 否                            |
   | allMatch() / anyMatch() / noneMatch() | ⚠️ 有时终止（取决于数据）       | boolean                      | ❌ 否                            |
   | forEach()                             | ❌ 不会终止                     | void                         | ❌ 否                            |
   | reduce()                              | ❌ 不会终止                     | T / Optional&lt;T&gt; / 其他 | ✅ 是                            |
   | collect()                             | ❌ 不会终止                     | T / Collection&lt;T&gt; 等   | ✅ 是                            |

5. 常见的流中间操作：

   ```java
   public Stream&lt;T&gt; filter(Predicate&lt;? super T&gt; predicate)                //过滤流  
   public Stream&lt;T&gt; distinct()                                                                //去重  
   public Stream&lt;T&gt; limit(long maxSize)                                                //规定大小  
   public Stream&lt;T&gt; skip(long n)                                                                 //跳过  
   public &lt;R&gt; Stream&lt;R&gt; map(Function&lt;? super T, ? extends R&gt; mapper)        //将流按照方法转换成另外的value  
   public &lt;R&gt; Stream&lt;R&gt; flatMap(Function&lt;? super T, ? extends Stream<? extends R&gt;> mapper)        //用来移除null之类的  
   public Stream&lt;T&gt; sorted()                        //使用自然排序  
   public Stream&lt;T&gt; sorted(Comparator&lt;? super T&gt; comparator)                //使用传入的排序方法  
   public Stream&lt;T&gt; peek(Consumer&lt;? super T&gt; action)                              //不影响流的情况下进行查看
   ```

6. 基本数据类型的流

   | **Method**                                  | **适用的 Primitive Stream**         | **说明**                                                     |
   | ------------------------------------------- | ----------------------------------- | ------------------------------------------------------------ |
   | OptionalDouble average()                    | IntStream, LongStream, DoubleStream | 计算流中元素的算术平均值                                     |
   | Stream&lt;T&gt; boxed()                     | IntStream, LongStream, DoubleStream | 将 primitive stream 转换为其对应的包装类型的 Stream&lt;T&gt; |
   | OptionalInt max()                           | IntStream                           | 获取流中的最大值（如果存在）                                 |
   | OptionalLong max()                          | LongStream                          | 同上                                                         |
   | OptionalDouble max()                        | DoubleStream                        | 同上                                                         |
   | OptionalInt min()                           | IntStream                           | 获取流中的最小值（如果存在）                                 |
   | OptionalLong min()                          | LongStream                          | 同上                                                         |
   | OptionalDouble min()                        | DoubleStream                        | 同上                                                         |
   | IntStream range(int a, int b)               | IntStream                           | 创建从 a（含）到 b（不含）的 IntStream                       |
   | LongStream range(long a, long b)            | LongStream                          | 同上                                                         |
   | IntStream rangeClosed(int a, int b)         | IntStream                           | 创建从 a（含）到 b（含）的 IntStream                         |
   | LongStream rangeClosed(long a, long b)      | LongStream                          | 同上                                                         |
   | int sum()                                   | IntStream                           | 计算流中所有元素的总和                                       |
   | long sum()                                  | LongStream                          | 同上                                                         |
   | double sum()                                | DoubleStream                        | 同上                                                         |
   | IntSummaryStatistics summaryStatistics()    | IntStream                           | 返回包含平均值、最大值、最小值、总和和计数的统计对象         |
   | LongSummaryStatistics summaryStatistics()   | LongStream                          | 同上                                                         |
   | DoubleSummaryStatistics summaryStatistics() | DoubleStream                        | 同上                                                         |

7. 不同类型流之间的转化方法

   | **Source Stream 类型** | **创建** Stream&lt;T&gt; | **创建** DoubleStream | **创建** IntStream | **创建** LongStream |
   | ---------------------- | ------------------------ | --------------------- | ------------------ | ------------------- |
   | Stream&lt;T&gt;        | map()                    | mapToDouble()         | mapToInt()         | mapToLong()         |
   | DoubleStream           | mapToObj()               | map()                 | mapToInt()         | mapToLong()         |
   | IntStream              | mapToObj()               | mapToDouble()         | map()              | mapToLong()         |
   | LongStream             | mapToObj()               | mapToDouble()         | mapToInt()         | map()               |

8. 基本数据类型的optional使用

   | **功能/方法**            | **OptionalDouble** | **OptionalInt** | **OptionalLong** |
   | ------------------------ | ------------------ | --------------- | ---------------- |
   | 获取基本类型值的方法     | getAsDouble()      | getAsInt()      | getAsLong()      |
   | orElseGet() 参数类型     | DoubleSupplier     | IntSupplier     | LongSupplier     |
   | max() / min() 的返回类型 | OptionalDouble     | OptionalInt     | OptionalLong     |
   | sum() 的返回类型         | double             | int             | long             |
   | average() 的返回类型     | OptionalDouble     | OptionalDouble  | OptionalDouble   |

9. 高级管道概念：暂放

## 10.2 Topics

### 10.2.1 System.in System.out

| **字段**                      | **类型**    | **说明**                 |
| ----------------------------- | ----------- | ------------------------ |
| [System.in](http://System.in) | InputStream | 标准输入（通常是键盘）   |
| System.out                    | PrintStream | 标准输出（通常是控制台） |
| System.err                    | PrintStream | 错误输出流               |

读取方式：

```java
Scanner sc = new Scanner([System.in](http://System.in));  
String name = sc.nextLine(); // ✅ 读取整行  
int age = sc.nextInt(); // ✅ 读取 int
//输出如果你不用println就需要手动flush才能输出
System.out.print("Hello");  
System.out.flush(); // 强制刷新输出
//System.out 是 final，不能被重新赋值（除非通过特殊方式如 setOut()）
//System.in在服务启动的时候就准备好监听输入了不需要等什么条件，一般System.in不能直接监听stream的输入，但是可以调用静态方法
//System.setIn(InputStream in)就能行。System.in.close()可以手动关闭监听输入。System.in的引用是 InputStream, 它的object是bufferedInputStream。System.input 是用来读字节的，但是其实字节和字符都可以读
Concole 在jvm启动后并且输入输出没有被重定向的时候才可以交互
System.out 不会抛异常 因为printStream本来就不会抛异常
System.out 也是在JVM启动的时候就准备好了，而不是要等到写控制台的时候
```

# 第十一章 Exceptions and Localization

## 11.1 Content and notes

1. Throwable有 Exception和Error子类，RuntimeTime继承自Exception，它是非受检异常。其他异常是受检异常，需要提前声明
2. 继承之后子类不能声明一个新的或者更广的异常
3. try-with-resources 用try 声明的资源只在try里面有效，到try结束就收回。
4. 在格式化数字的时候#表示省略，0表示用0补齐
5. try必须跟一个catch或者一个finally或者都跟，但是 try-with-resource 不需要，可以自行存在
6. 所有Throwable的子类 比如 Exception RuntimeException Error 都可以被throw 或者throws使用

# 第十二章 Modules

## 12.1 Content and notes

1. Module这一章是对 Java Platform Module System（JPMS）的测试，核心目的是更好地组织代码、增强封装性、提高安全性，并支持更好的可维护性

2. Java 模块是一种打包和封装的机制，是一个 .java 文件或 .jar 文件中的结构单元。明确声明了自己 **导出哪些包**、**依赖哪些模块**。需要 module-info.java 文件来描述模块的元信息。

3. module-info.java 文件每个模块的根目录下都必须有一个模块声明文件，形式如下：

   ```java
   module zoo.animal.feeding { 
       exports zoo.animal.feeding; 
       requires zoo.animal.care;
   }
   ```

4. 关键字：

   | **关键字**        | **用途说明**                  |
   | ----------------- | ----------------------------- |
   | module            | 定义模块的名称                |
   | exports           | 指定哪些包可以被其他模块访问  |
   | requires          | 声明依赖的其他模块            |
   | opens             | 指定哪些包允许通过反射访问    |
   | uses              | 使用服务接口（ServiceLoader） |
   | provides ... with | 提供服务的实现类              |

5. 依赖：

   1. **直接依赖**：通过 requires 显式声明。
   2. **传递依赖**：使用 requires transitive，使模块的依赖可以被其他依赖此模块的模块看到 module zoo.animal.care { requires transitive zoo.animal.feeding;}

6. 编译和运行

   ```java
   命令行常见操作
   
       
   javac -d mods/zoo.animal.feeding \\ src/zoo.animal.feeding/module-info.java \\ src/zoo.animal.feeding/zoo/animal/feeding/\*.java
   
   运行模块程序：
   
   java --module-path mods -m zoo.animal.feeding/zoo.animal.feeding.Main
   ```

7. 考试重点

   | **重点知识**                      | **示例**                                               | **考察方式**                   |
   | --------------------------------- | ------------------------------------------------------ | ------------------------------ |
   | 模块的声明语法                    | module zoo { exports a; requires b; }                  | 填空 / 判断语法合法性          |
   | exports 只能导出包                | exports com.example;（不能导出类）                     | 判断是否编译通过               |
   | requires 表示模块依赖             | requires zoo.animal.care;                              | 哪些模块需要在哪些模块之前编译 |
   | 模块名与目录结构匹配              | 模块名：zoo.animal.care => 路径：zoo/animal/care       | 编译顺序或运行路径选择题       |
   | transitive 与 static 修饰符的区别 | transitive 表示传递依赖；static 是编译时依赖（非强制） | 判断运行/编译是否出错          |
   | opens vs exports                  | opens 用于反射                                         | 判断可访问性                   |

   | **考点**                             | **错误理解**       | **正确理解**                                   |
   | ------------------------------------ | ------------------ | ---------------------------------------------- |
   | exports zoo.animals.Lion             | ❌ 错：不能导出类   | ✅ 只能导出包 exports zoo.animals;              |
   | requires zoo                         | ❌ 没有指定模块路径 | ✅ 模块必须在模块路径下编译并使用 --module-path |
   | 没有 module-info.java 也可以运行模块 | ❌                  | ✅ 不行，必须要有模块声明                       |

   | **Level** | **Within module code**                         | **Outside module**                                   |
   | --------- | ---------------------------------------------- | ---------------------------------------------------- |
   | private   | Available only within class                    | No access                                            |
   | Package   | Available only within package                  | No access                                            |
   | protected | Available only within package or to subclasses | Accessible to subclasses only if package is exported |
   | public    | Available to all classes                       | Accessible only if package is exported               |

   | **Artifact**               | **Part of the service** | **Directives required** |
   | -------------------------- | ----------------------- | ----------------------- |
   | Service provider interface | Yes                     | exports                 |
   | Service provider           | No                      | requires, provides      |
   | Service locator            | Yes                     | exports, requires, uses |
   | Consumer                   | No                      | requires                |

   | **Description**         | **Syntax**                                                   |
   | ----------------------- | ------------------------------------------------------------ |
   | Compile nonmodular code | javac -cp classpath -d directory classesToCompilejavac --class-path classpath -d directory classesToCompilejavac -classpath classpath -d directory classesToCompile |
   | Run nonmodular code     | java -cp classpath package.classNamejava -classpath classpath package.classNamejava --class-path classpath package.className |
   | Compile module          | javac -p moduleFolderName -d directory classesToCompileIncludingModuleInfojavac --module-path moduleFolderName -d directory classesToCompileIncludingModuleInfo |
   | Run module              | java -p moduleFolderName -m moduleName/package.classNamejava --module-path moduleFolderName --module moduleName/package.className |
   | Describe module         | java -p moduleFolderName -d moduleNamejava --module-path moduleFolderName --describe-module moduleNamejar --file jarName --describe-modulejar -f jarName -d |
   | List available modules  | java --module-path moduleFolderName --list-modulesjava -p moduleFolderName --list-modulesjava --list-modules |
   | View dependencies       | jdeps -summary --module-path moduleFolderName jarNamejdeps -s --module-path moduleFolderName jarNamejdeps --jdk-internals jarNamejdeps -jdkinternals jarName |
   | Show module resolution  | java --show-module-resolution -p moduleFolderName -m moduleNamejava --show-module-resolution --module-path moduleFolderName --module moduleName |
   | Create runtime JAR      | jlink -p moduleFolderName --add-modules moduleName --output zooAppjlink --module-path moduleFolderName --add-modules moduleName --output zooApp |

   | **Property**                                                 | **Named**                      | **Automatic** | **Unnamed**        |
   | ------------------------------------------------------------ | ------------------------------ | ------------- | ------------------ |
   | Does a \*\*\*\* module contain a module-info.java file?      | Yes                            | No            | Ignored if present |
   | Which packages does a \*\*\*\* module export to other modules? | Those in module-info.java file | All packages  | No packages        |
   | Is a \*\*\*\* module readable by other modules on the module path? | Yes                            | Yes           | No                 |
   | Is a \*\*\*\* module readable by other JARs on the classpath? | Yes                            | Yes           | Yes                |

8. 

## 12.2 Topics

### 12.2.1 JPMS

**top-down**：上层先模块化，依赖可暂放 classpath/module-path（自动模块或 unnamed）

**bottom-up**：先模块化底层库，逐步模块化上层应用

**automatic module**：无 module-info.java 但放在 module-path

**unnamed module**：放在 classpath 上的 legacy jar/class

unnamed module 的class 可以访问任何module中export的类

named module的class就要麻烦一下，如果他需要使用unamed 的话要先把unamed放到module path里变成auto然后通过require去访问

automatic module 可以获取unnamed module, 只要添加-classpath参数就行

简写规则， --module -m    / --module-path -p   /   --describe-module -d  其他的都没有简写

如果是使用service可能会用到 uses requires. 但是如果是提供service 可能会用到 provides requires

你想要load一个module进来，你可以直接找到这个jar或者找到jar所在的path， 然后使用module-path的几种形式load进来就行

# 第十三章 Concurrency

## 13.1 Content and notes

1. 创建线程：

   ```java
   new Thread(() -> System.out.print("Hello")).start();
   Runnable printInventory = () -> System.out.println("Printing zoo inventory");
   new Thread(printInventory).start();
   
   // Provide a Runnable object or lambda expression to the Thread constructor.  
   // Create a class that extends Thread and overrides the run() method.
   
   ```

2. 并发API：

   ```java
   ExecutorService service = Executors.newSingleThreadExecutor();
   service.execute(printInventory);
   service.execute(printRecords); // 同一个executorService 会串行执行 也就是说不管有几个任务只多创建一个线程
   ```

3. shutdown() 方法只是不会新建task去执行，不会中断已经注册执行的task

4. ExecutorService 无法使用try with resourcessubmit() 跟execute（） 不一样，会返回一个future对象让我们知道执行情况。run()方法并不会新起一个线程

5. 对于单个线程的创建，Executors.newSingleThreadExecutor(); 这里很可能用create,get之类的前缀来迷惑你

6. synchronized 不能用在class上，只能用在方法或者代码块上

7. 使用callable的原因： 你想获取执行结果，或者你想控制抛出受检异常

8. 仅从Executor 无法判断会创建多少个线程，它的构造方法可能创建一个也可能创建无数个

9. 如果想要延迟执行任务，选择使用 ScheduledExecutorService

# 第十四章 I/O

## 14.1 Content and notes

1. 基础类和用法：File比较简单，Path是其NIO的平替，Files里有很多静态方法可以操作Path的内容

   ```java
   File zooFile1 = new File("/home/tiger/data/stripes.txt");Path zooPath1 = Path.of("/home/tiger/data/stripes.txt");Path zooPath3 = Paths.get("/home/tiger/data/stripes.txt");//Files 是辅助类，用来判断文件是否存在之类的
   Files.exists(zooPath1);//zooFile1.toPath() path.toFile() 可以很好的转换
   Path zooPath1 = FileSystems.getDefault().getPath("/home/tiger/data/stripes.txt");
   ```

2. NIO

   1. 执行操作的时候有一些可选参数比如NOFOLLOW_LINKS，如果文件移动的时候默认会跟随符号路径找到真实文件去move，但是你加了这个参数的话就不会跟随符号路径而是直接把符号迁移  

   2. Path是不可变的所以执行某些操作会报错，但是有很多方法可以变相改变他的值比如：Path.of("/zoo/../home").getParent().normalize().toAbsolutePath(); 

   3. 相关方法：

      ```java
        path.getNameCount() path有及部分组成path.getName(0) path的这一部分的名字path.subPath(0,3) path的拆分,将012这三部分path拆出来path.getRoot() 返回根目录path.getFileName() 返回文件名path.getParent() 返回文件名前面的目录
      
        path1.resolve(path2) 把2拼接到1后面，但是加入path2是绝对路径那么就直接返回这个绝对路径
      
        path1.relativize(path2) 计算相对路径，注意当前文件也要算一层路径。两个要么都是相对要么都是绝对不然报错
      
        p1.normalize() 把路径中没用的部分去掉比如. 和 ..path.toRealPath() 去掉没用的部分转换符号链接最后得到成真实的地址
      
        Files.createDirectory（）创建目录，如果无法创建抛异常
      
        Files.createDirectories()创建目录，如果中间有不存在的就一并创建
      
        Files.copy(filePath, targetPath); 复制文件，如果想要替换文件记得加上REPLACE_EXISTING 否则会抛异常
      
        Files.move(filePath, targetPath); 移动文件，如果想要替换文件记得加上REPLACE_EXISTING 否则会抛异常
      
        Files.delete(filePath) 如果要删除目录则目录必须为空否则拋错
      
        Files.deleteIfExists(filePath) 方便判断是否存在，删除成功或失败返回bool
      
        Files.isSameFile(path1,path2) 是否是同一个文件，如果有链接到同一个文件那么返回相同
      
        Files.mismatch(path1,path2) 比较的是文件内容，返回第一个不匹配的位置索引或者相同的话返回-1
      ```

3. IO流:

   1. 区分字符流字节流，低级流和高级流，通常是高级流嵌套低级流使用以强化处理，对于考试，唯一需要熟悉的低级流类是对文件进行操作，其余非抽象流类均为高级流
   2. InputStream OutputStream Reader Writer 都是抽象类，然后前面加上File的类都是低级流，其他就是高级流。这四个低级类分别有read方法和write方法//可以单字节读取，batch字节读取，字符俺行读取，print俺行读取10: void copyStream(InputStream in, OutputStream out) throws IOException {11: int batchSize = 1024;12: var buffer = new byte\[batchSize\];13: int lengthRead;14: while ((lengthRead = in.read(buffer, 0, batchSize)) > 0) {15: out.write(buffer, 0, lengthRead);16: out.flush();17: }
   3. Files.readAllLine readString readAllBytes 可以用来读取Path文件输入，但是会一次性把整个文件加载进内存造成负载太大，此时可以使用Stream &lt;String&gt; s = Files.lines（path） 按行读入内存 注意readAllLine返回string而lines返回stream
   4. 序列化Serializable 该类必须标记为可序列化。类的每个实例成员都必须是可序列化的、标记为transient的、或在序列化时具有空值。ObjectInputStream readObject()用于反序列化 ObjectOutputStream writeObject()用于序列化 序列化类中定义的构造函数和任何实例初始化都会被忽略。Java 只会调用类层次结构中第一个不可序列化的父类的构造函数

4. 用户输入

   **控制台交互**：优先用 Console 类（安全），其次 Scanner。

   **文件交互**：NIO.2 的 Files 类更现代。

   **输入验证**：必须处理异常（如 NumberFormatException）。

   **本地化**：适应不同地区的日期、数字格式。

5. 高级API

   ```java
   public boolean markSupported(); //不同的stream可能不一定支持mark 检查流是否支持标记/重置。
   public mark(int readLimit); //打标记 设置标记位置（需指定有效期 readLimit）
   public void reset();// 回到标记重新读 回到标记位置（若标记失效则抛异常）
   public long skip(long n); //跳过指定数量的数据（适用于大文件或网络流）
   
   Files.isDirectory 检查路径是否指向目录
   
   Files.isSymbolicLink 检查路径是否指向普通文件
   
   Files.isRegularFile 检查路径是否指向符号链接
   
   Files.isHidden 查文件是否被系统标记为隐藏文件
   
   Files.isReadable 检查当前进程是否有读取权限
   
   Files.isWritable 检查当前进程是否有写入权限
   
   Files.isExecutable 检查当前进程是否有执行权限
   
   BasicFileAttributeView DosFileAttributeView PosixFileAttributeView 是三种视图，在大量访问文件属性的时候用他们可以节省资源
   
   var path = Paths.get("/turtles/sea.txt");BasicFileAttributes data = Files.readAttributes(path, BasicFileAttributes.class);
   
   System.out.println("Is a directory? " + data.isDirectory());
   
   System.out.println("Is a regular file? " + data.isRegularFile());
   
   System.out.println("Is a symbolic link? " + data.isSymbolicLink());
   
   System.out.println("Size (in bytes): " + data.size());
   
   System.out.println("Last modified: " + data.lastModifiedTime());//也可以更新attributes
   BasicFileAttributeView view = Files.getFileAttributeView(path, BasicFileAttributeView.class);BasicFileAttributes attributes = view.readAttributes();
   
   view.setTimes(lastModifiedTime, null, null);
   
   下面是两种递归检索目录和文件的方法public static Stream&lt;Path&gt; walk(Path start, FileVisitOption... options) throws IOExceptionpublic static Stream&lt;Path&gt; walk(Path start, int maxDepth, FileVisitOption... options) throws IOException
   ```

# Chapter 15 JDBC

## 15.1 Content and notes

1. 首先记住一个简单的例子：

   ```java
   public class MyFirstDatabaseConnection {
   	public static void main(String[] args) throws SQLException {
   		String url = "jdbc:postgresql://localhost:5432";
           try (Connection conn = DriverManager.getConnection(url);
   		PreparedStatement ps = conn.prepareStatement("SELECT name FROM exhibits");
   		ResultSet rs = ps.executeQuery()) { 
               while (rs.next())
               System.out.println(rs.getString(1));
           }
       }
   }
   ```

2. 如果DriverManager 创建连接的时候找不到对应的驱动就会抛出SQLException

3. Statement PreparedStatement CallableStatement

   | **特性**     | **Statement**                            | **PreparedStatement**                | **CallableStatement**                       |
   | ------------ | ---------------------------------------- | ------------------------------------ | ------------------------------------------- |
   | 类型         | 接口                                     | 接口                                 | 接口                                        |
   | 用途         | 执行普通 SQL（静态语句）                 | 执行带参数的 SQL（预编译）           | 调用数据库中的存储过程                      |
   | 安全性       | 低（容易 SQL 注入）                      | 高（防 SQL 注入）                    | 高（通常用于封装好的逻辑）                  |
   | 性能         | 较低（每次重新编译）                     | 高（预编译、可复用）                 | 高（依赖数据库对存储过程的优化）            |
   | 是否支持参数 | 否（需要拼接字符串）                     | 是，使用 setXXX() 方法               | 是，使用 setXXX() 和 registerOutParameter() |
   | SQL 编写方式 | "SELECT \* FROM user WHERE name = 'Tom'" | "SELECT \* FROM user WHERE name = ?" | "{call get_user(?)}"                        |

4. excute() executeQuery() executeUpdate(): 查询的时候使用 excuteQuery() 返回值是resultset。但是增删改的时候使用executeUpdate() 他们的返回值是 执行操作影响了多少条记录的int值。而execute返回一个bool值，他可以用于增删改查，然后通过getResultSet或者getUpdateCount去查返回来的数据

5. 传参数：不需要按顺序，把占位符的值通过set传进去然后执行 注意这里是第几个参数而不是index，从1开始的

   ```java
   String sql = "INSERT INTO names VALUES(?, ?, ?)";
   try (PreparedStatement ps = conn.prepareStatement(sql)) {
   	ps.setInt(1, key);
   	ps.setString(3, name);
   	ps.setInt(2, type);
   	ps.executeUpdate();
   }//还有setBoolean setDouble 等好几个方法
   ```

6. 获取结果集
   ```java
   ResultSet rs = ps.executeQuery()) {
       while (rs.next()) { //next很重要 第一是让游标到第一条数据，第二是判断是否真的有值 否则会报异常
           int id = rs.getInt("id");
           String name = rs.getString("name");
           idToNameMap.put(id, name);
       } // 这里也可以用getInt(1), getString(2) 来代表你要取第几个列的元素
   }
   ```

7. CallableStatement 和 存储过程
   ```java
   String sql = "{call read_e_names()}";
   try (CallableStatement cs = conn.prepareCall(sql);
        ResultSet rs = cs.executeQuery()) {
        while (rs.next()) {
           System.out.println(rs.getString(3));
       }
   }//传入参数var sql =“{call read_names_by_letter(?)}”；
   
   cs.setString("prefix", "Z");//输出 ？=是可选的帮助提高可读性
   var sql = "{?= call magic_number(?) }";
   cs.registerOutParameter(1, Types.INTEGER);
   cs.execute();
   System.out.println(cs.getInt("num"));//输入输出
   var sql = "{call double_number(?)}";
   try (var cs = conn.prepareCall(sql)) {
       cs.setInt(1, 8);
       cs.registerOutParameter(1, Types.INTEGER);
       cs.execute();
       System.out.println(cs.getInt("num"));
   }
   ```

8. 附加选项用法conn.prepareCall(sql, ResultSet.TYPE_FORWARD_ONLY, ResultSet.CONCUR_READ_ONLY); 只传一个的时候要注意并发

9. 事务
   ```java
   //关闭自动提交，然后根据你的事务去选择提交或者回滚
   
   conn.setAutoCommit(false);
   
   conn.commit();
   
   conn.rollback();//savepoint可以让你选择回滚的位置，记住回滚顺序，ABBASavepoint sp1 = conn.setSavepoint();
   
   conn.rollback(sp1);//处理完记得关闭所有资源
   ```

10. 关闭大范围的时候会顺带关闭小的，比如 关闭connection会关闭statement和resultset

11. JDBC 4.0 之后 Driver不需要再用 Class.forName 去加载进来了

12. 连接JDBC 直接用 DriverManager.getConnection 一次性把url,id,password三个参数都传进去， 或者 用Properties+url

# 第十六章 Other

1. array通常比arrayList 占用空间少
2. \`TreeSet\` 同时实现了：\`Set\` → \`SortedSet\` → \`NavigableSet\` ✅ NavigableSet 是有序的而且比SortedSet有更多可操作方法
3. SortedMap 是一个接口而不是类
4. 如果子类没有定义任何构造器，那么默认构造器会首先调用super() 而不是this()
5. 并不是数组声明的时候一定要说明大小，使用的时候再说明也行
6. 一个实例变量如果没有赋值，他的默认值数字类型是0或者0.0 注意没有f
7. 如果实例变量 char声明了没有赋值，打印的话是一个空格
8. 创建匿名类的时候是无法传递参数的，只适合简单类
9. 匿名内部类继承类的时候，初始化的时候是可以传递参数的，相当于构造一个父类对象。但是实现接口的时候就不行
10. 匿名内部类是可以继承外部类的
11. 你可以将嵌套类的构造方法变成private或者protected，但是不能对顶层类这样做
12. 对于顶层类来说 默认构造器的访问修饰符和类的访问修饰符一致





































---

# 第十七章 考题错题整理

## 1.四篇基础题

r**ecord 的构造函数有三种：标准，简洁，自定义。**

标准构造函数是编译器根据字段顺序默认提供的
如果在标准构造的基础上你像定制一些逻辑验证和异常处理的话可以定义一个简洁的
也可以自定义任意多个构造，但是这些构造必须调用this到其他构造最终到标准构造
只有标准构造可以使用this.name这样去赋值，整个record也就只有这一个方法可以赋值

---

**record其他知识：**

你可以定义以下三种记录类：

- 顶层记录类（top-level）：直接声明在包中；
- 嵌套记录类（nested record）：声明在类或接口内部；
- 局部记录类（local record）：声明在方法中。

记录类隐式为 final，你可以冗余写上 `final`，像sealed这种依托继承的关键词不可以修饰record

record不允许使用实例代码块，会破坏封装性，仅允许静态代码块

| 特性                                           | record 行为                             | 普通类行为                                       |
| ---------------------------------------------- | --------------------------------------- | ------------------------------------------------ |
| 默认构造函数                                   | 总是生成 canonical 构造函数             | 如果你定义了其他构造函数，默认构造函数就不会生成 |
| canonical 构造函数是否可泛型                   | ❌ 不可泛型                              | ✅ 可                                             |
| compact 构造函数能否声明 throws                | ❌ 不允许                                | ✅ 可以                                           |
| canonical  和 非 canonical 构造函数能否 throws | ✅ 允许                                  | ✅ 允许                                           |
| accessor 方法能否 throws 异常                  | ❌ 不允许                                | ✅ 允许                                           |
| 继承                                           | 继承 `java.lang.Record`，不能继承其他类 | 可继承任意类（除了 final）                       |
| 可否定义字段                                   | ❌ 实例字段不允许定义                    | ✅ 可自由定义                                     |

---

**创建线程的几种方式**

```java
//不推荐：简单但是不灵活，导致当前类无法继承其他类了，而且拿不到线程的返回值，无法抛异常class 
MyThread extends Thread {
    public void run() {
        System.out.println("Running in MyThread");
    }
}
Thread t = new MyThread();
t.start();
//最常用的基础写法，由继承类改为实现接口，但仍然无法获取返回值和抛异常
Runnable task = () -> System.out.println("Running");
Thread thread = new Thread(task);
thread.start();
//稍微复杂了一些，用FutureTask包装了一边Callable接口，好消息是可以获取返回值和抛异常了
Callable<String> task = () -> {
    Thread.sleep(1000);
    return "Result";
};
FutureTask<String> future = new FutureTask<>(task);
Thread thread = new Thread(future);
thread.start();
String result = future.get(); // 阻塞等待
//使用线程池:可控，性能好，但是需要你自己去管理线程池的生命周期
ExecutorService executor = Executors.newFixedThreadPool(2);
executor.submit(() -> System.out.println("Thread in pool"));
executor.shutdown();

```

注意：t.start 开启新线程，t.run只是在当前线程调用一个对象方法而已

---

**关于Console**

Console是JVM启动的时候由系统调用静态方法自动创建的
Console console = System.console();  注意它无法通过new进行创建
但是是否提供console对象还是要看运行环境，如果通过CMD之类的终端运行的大概都有console，但是如果通过IDE 一些后台服务和GUI运行的那么大概率没有console，就会返回null
console有一些 readLine readPassword 之类的方法来进行获取值，但是使用之前要判空，否则可能空指针异常

---

接口字段不可以被transient volatile和synchronized 修饰，前两者是因为接口字段属于静态字段，不参与对象序列化也因为是编译器常量，不可能被修改。而后者是因为它不可以修饰字段

---

Locale用来描述区域语言环境，比如有时候你要利用语言环境来决定时间日期之类的显示要按照什么国家和语言的习惯进行转换。它必须提供语言才能进行构造，它有三个构造方法都需要传入语言：

```java
Locale(String language);
Locale(String language, String country);
Locale(String language, String country, String variant)
```

---

关于钻石符号：

| 语法                 | 说明                                |
| -------------------- | ----------------------------------- |
| `<T>`                | 泛型类型声明                        |
| `List<T>`            | 泛型类型使用                        |
| `<?>`                | 通配符：表示“某种未知类型”          |
| `<? extends Number>` | 上界通配符：最多是 Number 或其子类  |
| `<? super Integer>`  | 下界通配符：最少是 Integer 或其父类 |

---

**内部类相关**

成员内部类在java 16之前是不允许拥有static方法和非静态方法的，但是16之后开始允许了

如何理解静态内部类 和 外部类之间的关系：

如果一个内部类被标注为静态，那就有意思了，这个静态内部类会被当做一个和顶级类一样的类进行处理。在编译之后会生成 OutterClass$InnerClass.class 从名字上有区分但是使用上几乎一样。但是它有一个比较有意思的点就是，JVM从设计上给静态内部类的访问权限开了后门，因为设计师认为虽然你现在是一个顶层类了，但是你依然是定义在别的类的内部的你和他们是一家人，所以你仍然应该包含对外部类的一定访问功能。然后就赋予了静态内部了访问外部类的private权限。同时静态类肯定可以访问外部类的static字段的（为什么呢？因为静态类是不依托于对象存在的，我虽然无法访问对象，但是非对象的东西我都可以访问）。

而对于非静态的内部类，它就很不一样了。它必须依托对象存在，因为没有static修饰的类就和普通成员一样，没有对象就没有对应的属性（或者这里说的成员类）。事实上JVM会在初始化非静态内部类的时候为它添加一个引用 final Outer this$0; 指向外部类，这就是为什么它可以访问成员变量和方法的原因（注意静态内部类是没有的）

而对于局部内部类和匿名内部类（匿名内部类其实就是没有名字的局部内部类，其他特点都很相似）在编译之后会生成单独的class文件，前者类似于OutterClass$LocalClass.class 而后者类似于OutterClass$1.class 同时注意局部内部类和匿名内部类在编译的时候JVM也会添加一个final Outer this$0; 用来获取当前类对象的引用，所以他们是可以访问对象的内容（甚至是private内容）的。前面是对于外部类对象属性的访问，那么对于外部方法的访问呢？这里比较有意思，因为它只能访问final或者effectively final的，因为编译初期，JVM会将方法的变量放到这个内部类当中，这样的话这些变量就必须不可变，否则会导致一些严重的后果

---

**JDK 命令相关**

最简单最基础的命令： `javac` / `java` / `jar`

`javac [options] [sourcefiles]`

`java [options] class [args...]`

`java [options] -jar jarfile [args...]`

`jar [options] [jar-file] [manifest-file] [input-files...]`

```test
编译
javac Hello.java								简单编译，输出class文件到当前目录
javac -d ~/myFolder Hello.java					编译，输出class文件到指定目录
javac -cp ~/myLibFolder/* Hello.java			编译并使用指定的类路径
还有其他常见选项比如 -verbose显示详细编译过程 -Xlint 开启所有警告方便调试 -encoding 指定源文件编码防止乱码等...
启动
java Hello										简单运行某个类
java com.example.Hello							运行某个类（含包名）
java -cp .:myLib/*:myOtherLib/* Hello			运行某个类并指定类路径
java --module-path myMods -m mymodule/com.example.Hello.Main 运行java程序并指定模块路径和模块主类
还有其他常见选项比如 -Xms 指定JVM初始堆大小 -Xmx 指定JVM最大堆大小 -XshowSettings 显示JCM的配置信息
打包
jar cf app.jar *.class 							打包并创建新的jar文件
jar uf app.jar NewClass.class					打包并更新已有jar
jar tf app.jar									列出jar的内容
jar cvf app.jar *.class							打包并列出详细信息
jar cfe app.jar com.example.Main *.class 		打包并指定可执行的主程序入口
引入模块之后传统的编译和运行命令可能会发生变化，比如以前通过classpath编译现在要通过modulepath编译了
java -p mods:lib -m my.module/com.example.Main
javac --module-path mods -d feeding feeding/zoo/animal/feeding/*.java feeding/module-info.java
java --module-path mods --module book.module/com.sybex.OCP  其中mods是依赖存放的地方，book.module是module的名字com.sybex是包名
java --module-path feeding --module zoo.animal.feeding/zoo.animal.feeding.Task  打jar成功之后可以在执行的时候直接使用jar
jar -cvf mods/zoo.animal.feeding.jar -C feeding/ .  打开feeding文件夹，把里面所有内容打包成指定的jar
```

虽然编译和运行就能跑起来程序了，但是你需要把他打成jar方便发送给其他人使用，一般来说打包不会把依赖的其他lib打进去，所以其他人在使用的时候仍然需要下载所有的lib才能运行，你当然也可以使用maven工具，这样pom文件里定义了哪些依赖是你需要去下载的。同时你也可以使用maven插件让其打包的时候打一个很大的胖jar，就是把所有的依赖都打进去，这样客户直接使用这个jar而不需要下载任何依赖。

现在的商业使用，镜像相比jar听到的更多，因为一个微服务会在docker file中定义一个镜像的具体细节，一个单纯的jar并不能直接运行，你可能会配置一些环境 数据库之类的，都在docker file中定义。 镜像才算是一个能够完全单独运行的总和

---

**Module相关**

上一话题之后可以再具体看一下module相关知识：

```
一次性编译两个package，这里使用同一个依赖位置，指明了两个package下的java文件，指定同一个module-info，这里的module-info里名字的定义可以理解为这两个package共同的部分，module-info认为是自己规定了这个名称下的所有包的封装配置
javac -p mods -d care care/zoo/animal/care/details/*.java care/zoo/animal/care/medical/*.java care/module-info.java

```

```text
module-info:
exports zoo.animal.talks.media;						普通的导出
exports zoo.animal.talks.content to zoo.staff;		导出到具体的包内使用，其他包无该权限
module并不破坏原有的访问修饰符特性，比如private导出了依旧无法在其他地方使用，package修饰符的导出后仍然无法在其他包使用
requires zoo.animal.feeding;						普通的依赖
requires transitive zoo.animal.feeding;				依赖它以及它自己的所有依赖，这里意味着使用传递式的依赖
注意重复依赖会报错，比如 requires A 之后又 requires transitive A 是不合法的
opens zoo.animal.talks.schedule; 					开放反射权限
opens zoo.animal.talks.media to zoo.staff;			对特定的包开放反射权限
如果你希望在暴露接口给别人的同时赋予别人动态寻找实现类的功能，可以使用 uses，目的是为了隐藏实现
module zoo.tours.reservations {
	exports zoo.tours.reservations; 
	requires zoo.tours.api;
    uses zoo.tours.api.Tour;
}
ServiceLoader<Tour> loader = ServiceLoader.load(Tour.class);
服务提供方 provides interfaceName with className 
provides zoo.tours.api.Tour with zoo.tours.agency.TourImpl;
```

```
java -p mods --describe-module zoo.animal.feeding		描述模块结构
java --list-modules										列出可用模块，现在JDK有至少70个
java -p mods --list-modules								列出该路径的可用模块，会包含JDK的
jar --file mods/zoo.animal.feeding.jar --describe-module描述模块，跟第一个稍有区别
jdeps zoo.dino.jar										描述依赖，在没有完全模块化的时候很有用
jdeps --jdk-internals zoo.dino.jar						描述依赖，提供调用内部API时候的更多细节
jmod 													用来跟jmod文件打交道，真实场景很少用，可能会涉及到一些原生功能
jlink --module-path mods --add-modules zoo.animal.talks --output zooApp		创建一个最小的发行版
```

不同类型的Module:
前面提到的这种拥有module-info 的module是*named module* 它在module path而不是 classpath上
*automatic module* 是指没有module-info 但是仍然在module path上的，java会自动为其使用一个module name。代码也会当作正常module来使用
*unnamed module* 不在module path 只在 classpath上 且通常是没有module-info的，如果有也会被忽略

自底向上：从最底下也就是不依赖其他jar的jar开始，为其添加module-info 写清楚requires和exports，从classpath移动到modulepath，往上进行
自顶向下：把所有jar都移动到module path下变成automatic module,然后从最顶端添加module-info写清楚requires/exports.往下进行

自底向上最简单，但是有时候你依赖了第三方的jar如果没有迁移就比较困难，这个时候用自顶向下比较方便

拆解项目：把依赖分类并且画出依赖关系，必要是要通过引入其他module来解除循环依赖，最后再进行迁移

---

**并发**

我们正常情况下编写的java程序，由程序员创建的就只有一个线程，它会启动并执行main方法。但是JVM会在后台启动一些守护线程进行垃圾回收内存分配之类的工作。那么一个java web程序是怎样的呢？拿springboot 举例，它内置了一个tomcat 服务器，这个服务器的线程池会为到来的每一个请求单独调配一个线程去响应，这个线程池默认是200的上限，你也可以调整这个值，但是要注意你的服务器的负载能力。由于不同的请求由不同的线程去处理，你要考虑好并发的问题，考虑共享资源的访问。对于有一些资源如果每一个线程进来都要创建的话可能造成负载重响应慢，这种时候我们可能考虑单例模式，就是创建一个对象供给所有线程使用，而spring的service和controller都是这样一种模式（想象一下如果不用单例，那么每个请求都要创建和初始化一套会浪费很多時間）所以处理共享资源的访问几乎是不可避免的

可以使用Thread.sleep(1000) 来暂停当前线程的执行，也可以通过myThread.interrupt(); 来将指定线程加入就绪状态

使用ExecutorService 接口可以方便我们管理线程

```java
ExecutorService service = Executors.newSingleThreadExecutor();
service.execute(printInventory);  //传入Runnable instance
service.shutdown(); service.shutdownNow(); //前者拒绝新增的task但是后者可以终端正在执行的task
service.execute(); service.submit();  //前者执行了无法获取结果但是后者会返回一个Future实例
service.isShutdown(); service.isTerminated(); //前者看你是否执行了shutdown但是后者会看你是否真的结束运行了
ScheduledExecutorService service = Executors.newSingleThreadScheduledExecutor(); //子接口用来执行定时任务
Excutors.newCachedThreadPool();		//按需创建无上限的线程并且可以复用，适合大量小任务
Excutors.newFixedThreadPool(int);	//创建定量的线程，如果新任务进来会进queue等待
Excutors.newScheduledThreadPool(int) //核心线程固定，非核心线程可以无限制创建
```

volatile 可以保证可见性和防止指令重排，但是无法保证原子性。原子性可以通过使用原子类来达到比如 AtomicInteger

synchronized 锁可以锁对象和方法，锁上之后所有线程都需要获取锁才能执行临界区代码。锁静态方法的话其实锁的是类对象，也就是说整个应用只能同时存在一个线程在调用该方法，锁实例方法则是锁的实例对象，也就是说如果两个线程使用两个实例对象的话，他们是有可能同时调用这个方法的。

Lock 和 synchronized 一样是作为一个锁的功能，但是他控制更简单，拥有跟多类似通知的功能，比如你想知道这个锁现在是否正在被使用。Ｌock锁是锁的对象还是类要看你的lock定义是什么样的，因为它锁的是lock对象，但是lock对象可能是类级别的也可能是实例级别的

```java
Lock lock = new ReentrantLock();try { lock.lock();
 // Protected code
} finally {
 lock.unlock();
}
```

并发使用的集合类：在多线程情况下你使用非并发类会导致读写一致性问题，就好像你i++的那个例子一样，为了不要出现一致性问题，通常有两种方式：
1.使用并发集合类：ConcurrentHashMap / ConcurrentLinkedQueue / ConcurrentSkipListMap / ConcurrentSkipListSet / CopyOnWriteArrayList / CopyOnWriteArraySet / LinkedBlockingQueue 其中skip可以理解为sorted，copyOnWrite 会在更新的时候创建一个新的copy
2.使用同步方法synchronizedCollection / synchronizedList / synchronizedMap 通常用这样的方法去获得并发集合类

---

**Stream**

1. Optional 首先是相关的几个方法，比如 opt.get() opt.ifPresent() opt.isPresent() 之类的

2. Stream的创建可以用工厂方法，也可以从collection之类的转换过来 比如 list.stream() list.parallelStream()

3. 流的使用是一次性的，一旦使用了归约就不能再进行链式调用了

4. 常见终端操作的方法和使用

   1. count() 计算流中元素个数，归约操作
   2. min( Comparator <? supert T> comparator ) 计算最小值，判断大小是通过传入的lambda来的，max同理
   3. findAny() / findFirst() 找到就返回这个元素，顺序流通常是第一个，并行流可能是任意一个
   4. anyMatch(Predicate <? super T> predicate)  通过你给的条件来判断是否有匹配项，allMatch noneMatch 同理
   5. forEach(Consumer<? super T> action) 对每一个元素执行操作
   6. reduce(BinaryOperator<T> accumulator) 最核心的归约操作，可以定义一个初始值
   7. collect(Collector<? super T, A,R> collector) 终端操作，将流输出到一个容器当中，通常跟Collector 工具类一起使用

5. 常见中间操作的方法和使用

   1. filter(Predicate<? super T> predicate)  通过true/false来过滤，会返回一个新的stream
   2. distinct() 去掉重复元素
   3. limit(long maxSize) 只保留前N个元素
   4. skip(long n) 跳过前N个元素只保留后面的元素
   5. map(Function<? super T, ? extends R> mapper) 对每一个元素进行转换，和foreach区别在于这个返回新流而foreach直接终止流
   6. flatMap( Function<? super T, ? extends Stream<? extends R>> mapper) 跟上一个比较像，用来处理元素包含集合或流的情况
   7. sorted(Comparator<? super T> comparator) 用来排序
   8. peek(Consumer<? super T> action) 对每个元素进行观察，和map的区别是这里是观察而map是改变

6. 关于基本数据类型流比如 IntStream 的出现是为了避免 Stream<Integer> 这样很大的装箱拆箱开销，而且这个流有更多的定制方法

7. 关于基本数据类型的Optional比如OptionalDouble 是同样的问题，可以看作高效版本的 Optional<Double> 。

8. 类似IntSummaryStatistics 这样的类 可以通过getCount() 这样简单的方式获得基本数据类型的求和/平均值/最大最小值等方法

9. 之前提到的Collector 有很多方法进行分组分类比如：

   ```java
   Map<String, List<Employee>> byDept = employees.stream().collect(Collectors.groupingBy(Employee::getDepartment));
   ```

---

**File**

**File** → 旧的类，路径抽象（`java.io`）但是不够灵活，方法很有限

**Path** → 新的路径抽象接口（`java.nio.file`） 相当于一个**路径抽象**，可以是文件、目录，甚至不存在的路径

**Paths** → 工具类，用来创建 `Path` 对象 提供**工厂方法**来创建 `Path` 对象

**Files** → 工具类，用来操作 `Path` 对应的文件 包含大量 **静态方法**，用来对 `Path` 表示的文件或目录执行操作。

File 有时指代的是 file & directory 因为他们在java中被视作一种东西就是file， 同样的方法即可以操作file又可以操作 directory。 而path 就是File & Directory 的抽象路径，注意不同的操作系统路径表达的区别

关于windows考题的斜杠问题：

Windows 系统路径用 `\`

Java 字符串里 `\` 要写成 `\\`

打印出来或 API 处理后还是单个 `\`

最推荐写法：在 Java 里用 `/`，跨平台无压力

```java
File file = new File("/home/test.txt");
Path path1 = Path.of("/home/test.txt");
Path path2 = Paths.get("/home/test.txt");
path.toFile();  file.toPath();

```

注意这个章节一般复数类是用来操作单数类的，比如： Paths Path / FileSystems FileSystem / Files Path /

给我的感觉很像是：以前用file这个类干很多事情，现在拆分成了： 抽象出来一个path， 操作都用Files 去操作path

Files 在操作的时候 可以添加一些 LinkOption参数 达到更灵活的目的比如 NOFOLLOW_LINKS 之类的

```java
path.resolve(""); 
path.getNameCount();
path.getName(0);
path.subpath(0,2);// 左闭右开
path.resolve("/first/second"); // 这里使用了绝对路径，所以最后结果还是这个路径,如果事相对路径就把它追加在path后面
path.relativize(path1); //这里有点抽象，注意我们当前在path的内部，考虑是否要回到path和path1同级目录
path.normalize();//去掉中间没意义的../之类的
path.toRealPath(); //可以将一些链接转化出来
//这些事path的方法，还有files的方法，太麻烦了懒得再去列出来了
```

接下来看字符流字节流



## 2.正篇

123先放着稍后复习

### 第四套

第一题 读题不仔细，选择选项的时候注意选择最合理的，虽然可能很多选项都是对的。sealed class A permits B 如果B不继承，则两个类都会编译出错

第五题 要注意经典的不可变类，instant 是不可变的，对它进行加减如果不重新赋值引用，原对象不会发生改变

第六题 考察RandomAccessFile 类，它它允许你对随机访问文件进行 读写操作 随机访问文件的行为就像存储在文件系统中的一个大型字节数组。在这个“数组”中有一个类似光标或索引的东西，称为 文件指针。文本末尾添加直接用length方法。它支持r/rw/rws/rwd

第七题 考察callable 相关，记住Future是接口所以new Future<String> 作为返回是错误的

第八题 考察垃圾回收，调用System.gc() 是告诉JVM可以垃圾回收了但不一定立刻执行。只要obj被重新赋值为null那么就意味着老的对象没有引用了，被标记为可回收

第十题 读题要仔细，接口无法实现接口，然后如果一个方法返回值是short但是你返回字面量0是合法的，虽然表面看起来没有强制转换要报错，但是字面量而且完全在short的区间内就会自动进行类型转换，不会报错。java17的接口支持 default/static/private/private static 不支持protected/final方法

第11题： 注意path.getName(1) 在计算的时候 C:// D:// 都不算，要从他们后面开始算0号元素

第12题：stream可能会考到 lambda使用外部变量求和，这个时候外部变量可能不是final或者effective final 就会编译错误

第13题 Collection.sort(param1,param2) 如果param2 不传就是说使用默认排序方法，调用集合的compareTo ，但是不同类型无法一起compareTo 会编译错误

第15题 强化 path.resolve方法，注意startsWith 传入string的话要完全匹配，不要忽略了 / 或者C:\\\\

第18题 module-info不能为空，如果缺失这个文件，并不会从class的名字去推断它，如果放在module path 就是automatic module 从jar的名字去推断。 module-info.java 同样会被编译成 class 文件

第19题 Arrays.compare(a,b) 如果b是a的前缀，那么compare会返回 他们之间差异个数的正值，mismatch 会返回第一个不同的index

第25题 静态内部类不需要先 new 外部类，有时候做题要考虑到 静态引入外部类的静态类，那么就更简单了 直接 new Inner() 

第27题， console的创建失败并不会报错只会返回null，所以不需要声明异常

第30题 **记得去复习day time 这个东西**

第31题 枚举的构造方法只能是私有的，因为要保持枚举常量的唯一性，不让其他人随意的构造枚举对象

第32题 a += (a = 4); 这样的题先展开再从前往后做，在做到赋值之前都不受赋值影响，赋值之后的就要受到赋值的影响了

第36题 boolean 不能作为switch表达式或者语句的判断（有点反直觉。。。）

第37题 RandomAccessFile 没有 writeString 方法，有 writeUTF方法

第38题 **强化一下 Locale 的知识**

第39题 考察 stream 的terminal 方法比如 foreach 如果没有这种方法，那么链式调用的中间方法都不会执行

第40题 stringbuilder 没有setCapacity 方法只有 ensureCapacity 方法

第41题 **重新复习bundle 相关知识**

第43题 初始化一个数组的时候你不能既指定 长度 又指定内容

第44题 case 后面跟的是一个值去作比较，而不是直接让你写表达式去作比较，写表达式会返回true 和false

第45题 stream 相关，有点意思，再看一遍

第47题 注意 module-info 里面不能有通配符*

第48题 跟32有点像，这种运算 先计算等号左边，再计算右边，如果右边有等号再计算其左边和右边，不要让右边的赋值影响到你对前面的判断

第50题 LocalDate 的打印是像这样的： 2022-02-14

第55题 考察 database 的 resultset 的方法 比如 getString getInt

### 第五套













































