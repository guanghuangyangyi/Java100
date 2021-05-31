# Java8新特性

### 特点
- 速度更快
- 代码更少（增加了新的语法Lambda 表达式）
- 强大的Stream API
- 便于并行
- 最大化减少空指针异常Optional

## Lambda 表达式
- Lambda 是一个匿名函数，我们可以把Lambda 表达式理解为是一段可以传递的代码（将代码像数据一样进行传递）
```java
//匿名内部类
TreeSet<String> ts2 = new TreeSet<>(new Comparator<String>(){
    @Override
    public int compare(String o1, String o2) {
        return Integer.compare(o1.length(), o2.length());
    }
});
//Lambda表达式
Comparator<String> com = (x, y) -> Integer.compare(x.length(), y.length());

TreeSet<String> ts = new TreeSet<>(com);
```
- Java8中引入了一个新的操作符 "->" 该操作符称为箭头操作符或 Lambda 操作符，箭头操作符将 Lambda 表达式拆分成两部分： 
左侧：指定了Lambda 表达式需要的所有参数 
右侧：指定了Lambda 体，即Lambda 表达式要执行的功能。
```java
/**
* 上联：左右遇一括号省
* 下联：左侧推断类型省
* 横批：能省则省
*/
//语法格式一：无参数，无返回值
() -> System.out.println("Hello Lambda!");

//语法格式二：有一个参数，并且无返回值
(x) -> System.out.println(x)

//语法格式三：若只有一个参数，小括号可以省略不写
x -> System.out.println(x)

//语法格式四：有两个以上的参数，有返回值，并且 Lambda 体中有多条语句
Comparator<Integer> com = (x, y) -> {
    System.out.println("函数式接口");
    return Integer.compare(x, y);
};

//语法格式五：若 Lambda 体中只有一条语句， return 和 大括号都可以省略不写
Comparator<Integer> com = (x, y) -> Integer.compare(x, y);

//语法格式六：Lambda 表达式的参数列表的数据类型可以省略不写，因为JVM编译器通过上下文推断出，数据类型，即“类型推断”
(Integer x, Integer y) -> Integer.compare(x, y);
```

## 函数式接口
- Lambda 表达式需要“函数式接口”的支持 
  函数式接口：接口中只有一个抽象方法的接口，称为函数式接口。 可以使用注解 @FunctionalInterface 修饰 
  可以检查是否是函数式接口

## 方法引用与构造器引用
### 方法引用：若 Lambda 体中的功能，已经有方法提供了实现，可以使用方法引用  
    （可以将方法引用理解为 Lambda 表达式的另外一种表现形式）

- 对象的引用 :: 实例方法名
```java
PrintStream ps = System.out;
Consumer<String> con = (str) -> ps.println(str);
con.accept("Hello World！");

Consumer<String> con2 = ps::println;
con2.accept("Hello Java8！");
```

- 类名 :: 静态方法名
```java
Comparator<Integer> com = (x, y) -> Integer.compare(x, y);

Comparator<Integer> com2 = Integer::compare;
```
- 类名 :: 实例方法名
```java
Function<Employee, String> fun = (e) -> e.show();
System.out.println(fun.apply(new Employee()));

Function<Employee, String> fun2 = Employee::show;
System.out.println(fun2.apply(new Employee()));
```
  注意：  
  	 ①方法引用所引用的方法的参数列表与返回值类型，需要与函数式接口中抽象方法的参数列表和返回值类型保持一致！  
  	 ②若Lambda 的参数列表的第一个参数，是实例方法的调用者，第二个参数(或无参)是实例方法的参数时，格式： ClassName::MethodName

### 构造器引用 :构造器的参数列表，需要与函数式接口中参数列表保持一致！
- 类名 :: new
```java
Supplier<Employee> sup = () -> new Employee();
System.out.println(sup.get());

Supplier<Employee> sup2 = Employee::new;
System.out.println(sup2.get());
```
### 数组引用
- 类型[] :: new;
```java
Function<Integer, String[]> fun = (args) -> new String[args];
String[] strs = fun.apply(10);
System.out.println(strs.length);

Function<Integer, Employee[]> fun2 = Employee[] :: new;
Employee[] emps = fun2.apply(20);
System.out.println(emps.length);
```
## Stream API
- Stream API 提供了一种高效且易于使用的处理集合数据的方式,使用Stream API 对集合数据进行操作，就类似于使用SQL 执行的数据库查询.
- 集合讲的是数据，流讲的是计算
    - Stream 自己不会存储元素。
    - Stream 不会改变源对象。相反，他们会返回一个持有结果的新Stream。
    - Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。
- Stream 的操作步骤
    - 创建 Stream

    - 中间操作
        - 多个中间操作可以连接起来形成一个流水线，除非流水线上触发终止操作，否则中间操作不会执行任何的处理！而在终止操作时一次性全部处理，称为“惰性求值”

        - 筛选与切片  

           方法|描述  
           --|:--  
           filter(Predicatep) | 接收Lambda ，从流中排除某些元素。  
           distinct() | 筛选，通过流所生成元素的hashCode() 和equals() 去除重复元素  
           limit(long maxSize) | 截断流，使其元素不超过给定数量。  
           skip(long n) | 跳过元素，返回一个扔掉了前n 个元素的流。若流中元素不足n 个，则返回一个空流。与limit(n) 互补    

        - 映射

           方法|描述  
           --|:--  
           map(Functionf) | 接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。
           mapToDouble(ToDoubleFunction f) | 接收一个函数作为参请按照2数，该函数会被应用到每个元素上，产生一个新的DoubleStream。
           mapToInt(ToIntFunction f) | 接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的IntStream。
           mapToLong(ToLongFunction f) | 接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的LongStream。
           flatMap(Function f) | 接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流

        - 排序

          方法|描述
          --|:--
          sorted()|产生一个新流，其中按自然顺序排序
          sorted(Comparatorcomp)|产生一个新流，其中按比较器顺序排序

    - 终止操作

        - 查找与匹配

            方法|描述
            --|:--
            allMatch(Predicate p) | 检查是否匹配所有元素
            anyMatch(Predicate p) | 检查是否至少匹配一个元素
            noneMatch(Predicatep) | 检查是否没有匹配所有元素
            findFirst() | 返回第一个元素终端操作会从流的流水线生成结果。其结果可以是任何不是流的值，例如：List、Integer，甚至是void
            findAny() | 返回当前流中的任意元素 
            count() | 返回流中元素总数
            max(Comparatorc) | 返回流中最大值
            min(Comparatorc) | 返回流中最小值
            forEach(Consumerc) | 内部迭代(使用Collection 接口需要用户去做迭代，称为外部迭代。相反，Stream API 使用内部迭代——它帮你把迭代做了)

    - 规约
        方法|描述
        --|:--
        reduce(T iden, BinaryOperator b) | 可以将流中元素反复结合起来，得到一个值。返回T
        reduce(BinaryOperator b) | 可以将流中元素反复结合起来，得到一个值。返回Optional<T>

    - 收集
      
      方法|描述
      --|:--
      collect(Collector c) | 将流转换为其他形式。接收一个Collector接口的实现，用于给Stream中元素做汇总的方法

备注：map 和reduce 的连接通常称为map-reduce 模式，因Google 用它来进行网络搜索而出名。

## 接口中的默认方法与静态方法

- Java 8中允许接口中包含具有具体实现的方法，该方法称为“默认方法”，默认方法使用default关键字修
- 若一个接口中定义了一个默认方法，而另外一个父类或接口中又定义了一个同名的方法时
  - 选择父类中的方法。如果一个父类提供了具体的实现，那么接口中具有相同名称和参数的默认方法会被忽略。
  - 接口冲突。如果一个父接口提供一个默认方法，而另一个接口也提供了一个具有相同名称和参数列表的方法（不管方法是否是默认方法），那么必须覆盖该方法来解决冲突

## 新时间日期API

- LocalDate、LocalTime、LocalDateTime
- Instant 时间戳
- Duration (时间间隔)和Period(日期间隔)

## 其他新特性

- Optional