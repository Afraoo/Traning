

### Java核心技术卷1

> 用来记录学习此书时的笔记和感想



##### 第一章  Java程序设计概述

主要讲述了Java语言的特性（安全），其中映像最深的是“解释型”，也就是平常所说的可移植性的原因：只要在由解释器的电脑上，都可以运行`.class`文件，该文件是通过`.java`文件编译生成的，由字节码构成。其次是关于Java的发展历程以及关于版本的说明。

##### 第二 章  Java程序设计环境

- jdk安装与环境变量配置

- 使用命令行中的`javac `来编译`Welcome.java`文件从而生成`Welcome.class`文件，再使用`java` 来运行`.class`文件，从而在控制台中输出程序的运行结果`Welcome to Java!`。

  > .class文件也就是字节码文件，该类文件可以在任何具有解释器的设备上运行。（可移植性）

- 使用集成开发环境来进行java开发（个人感觉VScode更灵活，插件很多很方便，IDEA更强大好用，都比Eclipse好）

- 写了一个图形化界面显示图片的小程序（感觉跟Android客户端开发部分内容有点像比如Activity中的按钮等的添加和相应事件）：

  ```java
  /**
   * This is a test.
   * @author Xilei Lin
   */
  package ImageView;
  
  import javax.swing.*;
  import java.awt.*;
  import java.io.File;
  
  public class ImageViewer {
  
      public static void main(String[] args){
  
          EventQueue.invokeLater(() -> {
              JFrame frame = new ImageViewerFrame();
              frame.setTitle("ImageViewer");
              frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
              frame.setVisible(true);
          });
      }
  }
  
  class ImageViewerFrame extends JFrame{
  
      private JLabel label;
      private JFileChooser chooser;
      private static final int DEFAULT_WIDTH=300;
      private static final int DEFAULT_HEIGHT=400;
  
      public ImageViewerFrame(){
  
          setSize(DEFAULT_WIDTH, DEFAULT_HEIGHT);
  
          label = new JLabel();
          add(label);
  
          chooser = new JFileChooser();
          chooser.setCurrentDirectory(new File("."));
  
          JMenuBar menuBar = new JMenuBar();
          setJMenuBar(menuBar);
  
          //在菜单栏中添加File菜单
          JMenu menu = new JMenu("File");
          menuBar.add(menu);
          
          //将Open选项加入File菜单中
          JMenuItem openItem = new JMenuItem("Open");
          menu.add(openItem);
          openItem.addActionListener(event -> {
  
              int result = chooser.showOpenDialog(null);
              if(result == JFileChooser.APPROVE_OPTION){
  
                  String name = chooser.getSelectedFile().getPath();
                  label.setIcon(new ImageIcon(name));
              }
          });
  
           //将Exit选项加入File菜单中
          JMenuItem exitItem = new JMenuItem("Exit");
          menu.add(exitItem);
          exitItem.addActionListener(event -> {System.exit(0);});
    }
  }
  
  ```

         在该程序中，需要注意的是`Item.addActionLisener()`这个方法，该方法对针对于 *item* 的点击动作进行监听，当监听到该操作时，则执行括号中的处理。
        
         其次，关于在Java中的三种注释方式：

  - //
  - /*   ...... */
  - /**  ...... */     该种方式常用于对某个代码块进行说明，比如作者等信息也可以在其中进行声明。

- 关于applet的使用与说明。由于之前曾出现过的安全问题，导致现在的applet使用的情况并不是特别理想。



##### 第三章 Java的基本程序设计结构

1. Java程序简介

   - 类就是代码容器，是代码存放的单位，库都是由类组成的

   - Java的类名必须以字母开头，区分大小写

   - 必须有一个类的类名与文件名一致，该类的访问修饰符必须为public。其中必须包含程序入口main方法，为了能够正常执行该程序，该方法的修饰符必须为public

     

2. Java的8种基本数据（4整型，2浮点，1字符，1布尔）：

   - ` int   short   long   byte `

   - ` float   double `

   - `char`

   - ` boolean`

     > `boolean` 的值只有两个：`false（1）`，`true（0）`，与C++正好相反

     

3. 数值计算机中不允许有任何舍入误差时，使用`BigDecimal`类

   > 该类在`Package java.math`中，同在的还有`BigInteger`类

   在`BigInteger (Bigdecimal)`类中，加减乘除运算通过调用以下方法实现：

   - `a.multiply(b)`： `a * b`

   - `a.divide(b)`：`a/b`

   - `a.add(b)`：`a + b`

   - `a.subtract(b)`：`a - b`

   - `a.mod(b)`：`a % b`，取余

     当然，也可以使用`BigInteger.valueOf(a)`来讲a转换成为`BigInteger`。

   

4. `const `和 `goto `都是作为Java中的保留关键字，并不使用。

   - `strictfp`关键字是用来修饰方法的，表明在该方法中使用严格的浮点计算来得到相关结果。

   

5. 声明一个变量之后，一定要对其进行显式的初始化。比如：

   ```java
   int age = 22;
   ```

   

6. 使用`final`来修饰常量，修饰变量的话则表明该变量只能被赋值一次。

7. 使用static final来修饰类常量，这样类中的所有方法都可以使用该常量。例如：

   ```java
   public static final int age = 22;	
   ```

8. 浮点数除以0，结果无穷大（Infinity）；整数除以0，则会报以下错误：

   ```java
   java.lang.ArithmeticException
   ```

9. Math中的相关方法：

   ```java
   import static java.lang.Math.*;//注意是static
   ```

   - `sqrt(double x)`： 得到平方根,返回值是double类型

   - `pow(double x, double a)`：得到x 的a次方，返回值是double类型

   - `floorDiv(int x, int y)`

     - 当`x`和`y`符号相同时，则该方法返回的结果与`x/y`相同
     - 当符号不同时，则该方法得到的结果为：对`x`除以`y`的商向下取整

   - `floorMod(x, y)`：该方法得到结果与`floorDiv(int x, int y)`有以下关系：

     ==`floorMod(x, y) + floorDiv(int x, int y) * y = x `==

   - `atan`: 即`arctan`，得到结果范围为 `-pi/2 ~ pi/2`

   - `atan2(x,y)`：将矩形坐标`(x, y)`转化为对应的极坐标`(r, theta)`中的角度`theta`，范围为`-pi ~ pi`

   - `random()`：返回一个0~1之间（含0不含1）的随机浮点数

     

10. 数值转换：

    - 优先级：`double > float > long > int`

    - 强制转换示例 :

      > *注意：*double进行强制转换为int，通过将小数部分截掉得到。

      ```java
      double x = 99.9;
      int y = (int) x;
      ```

      

11. 关于位与boolean运算符：

    - `a && b`：当第一个表达式a为false时，第二个表达式b就不用计算了，结果为：false

    - `a || b`：当第一个表达式a为true时，第二个表达式b不用计算了，结果为：true

    - 关于三元运算符`condition ? expression1 : expression2`

      当condition为真时，结果为`expression1`中的值，否则为`expression2`中的值。

    - `>>>`用0填充高位，`>>`用符号填充高位

    - `+=`和`-=`为右结合运算符，因此：

      ```java
      a += b += c;
      a += (b += c);//优先级顺序
      ```

      

      

12. 字符串

    > 预定义类String，任何一个Java对象都可以转换成字符串

    - `subString(a, b)`：从大字符串中得到一个子串，子串范围：`a <= str <b`

    - `join(分隔符，字符串)`： 将多个字符串用分隔符分开，其中，多个字符串之间使用逗号隔开

    - 检查字符串既不是空串（使用equals()）也不是null时，要先检查null，因为在null上调用方法会报错。

      

13. Scanner

    > 这个部分需要多去写一些代码来进行练习、体会，Scanner中的next开头的方法，以及关于控制台输入的字符是否合法的判断，都需要自己多去通过代码摸索练习。

           通过以下语句来将标准输入流`System.in`与`Scanner`对象进行关联，从而可以获得控制台的输入数据。

    ```java
    Scanner in = new Scanner(System.in);
    ```

    - `printf("%8.2f",x)`：以8个字符的宽度、保留两位小数的格式输出`x`(s字符串、f浮点数、d整型)

    - *文件名包含路径时，要多加一个反斜杠*

      

14. 关`switch`的`case`标签，它可以是：

    - char、byte、short、int

    - 枚举常量（大写字母）

    - 字符串字面量（SE 7开始），例如：

      ```
      switch(str.toLowerCase()){
          case "yes":
      }
      ```

      

15. 数组中使用到的`Arrays`类（*注意：后面有个 s*）

    - `Arrays.toString(a)`：当a为一维数组时，此句将该数组元素放在中括号中输出，当a为二维数组时，则以行为单位输出，每行的数据使用中括号括起来。

    

    

    ##### 总结

    ​       在这三章中，虽是关于Java的基础知识，但是仍然很重要。特别是关于Scanner和String的一些知识讲解，这两个类对于以后的编程来讲会经常用到。因此 ，需要下来多练习这两个类其他方法的使用以及在使用时需要注意的地方。

    ​       很多书面上的知识还是需要自己去使用代码来实现练习，才能理解得更加深刻和到位。特别还需要注意一点的时，在编程的时候永远不要忘了边界条件等，书上的示例只是简单地展示并没有过多的去注意这方面。但是这些问题是一直存在且需要处理的，没有提及并不代表就不做相关处理或是不重要。最开始自己也忘了去考虑这些，直到练习到后面渐渐发现。最后，开发文档真的很好用，讲的很详细，需长期使用：https://docs.oracle.com/javase/8/docs/api/allclasses-noframe.html







##### 第四章 对象与类

> 对象都存储在堆中

1. 对象的三种特性，其中

   - 对象的行为通过方法来体现

   - 对象的状态通过实例域来体现

   - 对象变量是引用了一个对象

      ​        好的封装，应该只通过对象的方法来进行交互；而对象状态的改变必须是通过调用方法来实    现。当一个文件中有多个类时，除了和文件名相同的public公有类之外，其他非公有类中的实例域都应该是除了自身的方法可以访问外，其他类的方法都是不可读写的。但是可以提供方法来操作该类的对象以及存取它们的实例域。

   - `Accessor Method`：访问器方法，如getter

   - `Mutator Method`：更改器方法，如setter和Raise Salary方法。

     > 不是所有的域中的数据都需要设置更改器方法

   

2. 关于`LocalDate`

   > 输出的格式为：年-月-日

   - `now()`：获得当前的年月日

   - `of(1993,1,1)`：将括号中的数值构造为一个`LocalDate`对象

   - `getMnoth()`：使用该方法时得到的时月份的英文单词，若要获得具体数值，则应使用`getMonthValue()`，需要注意

   - `plusDay(int num)`：在当前日期上加上（`num`为负数时则是减去）括号中天数后的日期

   - `DayOfWeek`：该类是代表一周七天的枚举类型,使用`getValue()`可以得到某一天对应的编号，1代表周一，7代表周日。使用示例：

     ```java
     LocalDate localDate = LocalDate.now();
     DayOfWeek dayOfWeek = localDate.getDayOfWeek();
     ```

     

3. 类的访问权限

   - 一个类的方法可以访问这个类任意对象的私有域

   - public、protected、private

     

4. 隐式参数

   使用关键字`this`来表示。下例中`number007`为一个隐式的参数，5为显式参数。

   ```java
   number007.raiseSalary(5);
   ```

   

5. 构造器

   构造器与类同名，使用`new`操作符构造类的对象时，构造器会运行

   > 不能在构造器中定义与实例域重名的局部变量

6. ###### 对象构造

   构造对象很重要，其中有以下几种方法可以编写构造器：

   + 重载

     多个方法中，如果名字相同，但是参数的类型与数量不同，则产生了重载。但是仅仅方法的返回类型不同则不属于重载，因为返回类型不在方法签名中。

   + 域没有显式赋值则会默认设置（0，false或null），但是方法中的局部变量必须明确初始化。初始化可以

   + 在一个构造器中可以使用this来调用另一个构造器

   + 对象初始化块和静态初始化块，都在域定义之后进行。

   + 在C++中人工回收资源使用的是显式的析构器，而在Java中使用的是f inalize()方法。

   ###### 总结

   ​        在本小节中主要讲的是构造器的几种编写方法，其中最常用也是最重要的就是重载，在继承父类的子类中，也经常会为了扩展父类的方法对其进行重载。当一个类中存在不止一个构造器时，可以在构在其中使用this来调用另一个构造器。关于数据域，若没有显示初始化则会默认将域初始化为默认值（数值为0，布尔为false和对象为null），String类型的数据也是String类的对象，所以在此情况下字符串也会默认初始化为null。

   ​        编写代码的时候，无论方法名还是参数名，应该尽量为提高代码的可读性而设置，代码块的逻辑与层次也要分明。

   

7. 包

   + 每个包中都是一个独立的类集合，通过包可以确保类的唯一性。
   + 当两个包中有相同名字的类（如Date）时，如果只使用其中一个包中的类时，在import时写出完整的包+类名即可；如果两个类都用，则在程序中每个类前写出具体的包名。
   + public修饰的类，可以被任意类使用。未加任何修饰符时，则可以被其所在包的所有方法访问。

   总结

   ​        进行项目开发时，应当明确每个类所在的包，以及每个类、方法和域中数据的访问权限。通过包来对代码进行管理，并且要确保每个类在正确的包中，以免虚拟机找不到类报错。

   

8. 类路径

   ​        在配置JDK环境的时候classpath的作用是为了设置虚拟机搜寻类的起始点，以及编译器进行编译的时候搜寻导入和引用的类，从而正确编译文件。

   

9. 文档注释

   ​        通过javadoc工具生成，在第二章中提到的三种注释方法的第三种`/**`开头`*/`结尾的注释即是文档注释。类与方法的文档注释应该分别在import之后类定义之前、方法之前。

   ​        包注释，则是在包目录中添加一个以package.html或者package-info.java命名的文件。html中的<body></body>中的内容会被抽取出来；而.java中，需要在包语句之后跟随一个文档注释。

10. + 数据私有化
   + 一定要记得初始化数据
   + 不要使用过多的基本数据，可以使用类来代替
   + 优先使用不可变的类
   + 类名和方法名要能体现其作用
   + 复杂的类可以拆解为几个简单的类

