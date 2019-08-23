---
title:  "Java Notes"
date:   2019-08-23 00:00:00
categories: [Java]
tags: [Java, notes]

---



1. ### 反射 Reflection

   * 获取Class实例的三种方式：

     * String.class

     * String s = “123”

       s.getClass()

     * Class.forName("java.lang.String")

   * Class 实例和instanceof 区别：instanceof可以判断子类型

   * 为什么需要反射：反射允许静态语言在运行时检查修改程序的结构和行为

   * 访问字段：

     * 通过Class实例的方法获取Field对象：

       1. getField(name): 获取某个public的field (包括父类)
       2. getDeclaredField(name): 获取当前类的某个field (不包括父类)
       3. getFields(): 获取所有public的field (包括父类)
       4. getDeclaredFields(): 获取当前类的所有field (不包括父类)

     * 通过Field实例可以获取field信息：

       getName(), getType(), getModifiers()

     * 通过Field实例可以获取或设置值：f.get(n), f.set(n, 456)

     * 通过设置setAccessible(true)来访问非public字段

   * 调用方法:

     * 通过Class实例的方法可以获取Method的实例：

       getMethod/getMethods/getDeclaredMethod/getDeclaredMethods

     * 通过Method实例可以获得方法信息：

       getName/getReturnType/getParameterTypes/getModifiers

     * 通过Method实例可以调用某个对象的方法:

       Object invoke(Object instance, Object... parameters)

     * 通过设置setAccessible(true)来访问非public方法

   * 调用构造方法：

     * Class.newInstance() 只能调用无参数的构造方法

     * Constructor对象封装了构造方法的所有信息

     * 通过Class实例的方法可以获取Constructor的实例：

       getConstructor/getConstructors/getDeclaredConstructor/getDeclaredConstructors

     * 通过Constructor实例可以创建实例对象：newInstance(Object... parameters)

     * 通过设置setAccessible(true)来访问非public构造方法

   * 获取继承关系：

     * getSuperclass()
     * getInterfaces()
     * 通过Class对象的isAssignableFrom()方法可以判断一个向上转型是否正确

   

2. ### 注解 Annotation

   * 注解是放在Java源码的类、方法、字段、参数前的一种标签，本身不影响程序逻辑

   * 注解可以配置参数，没有指定配置的参数使用默认值

   * 如果参数名称是value，可以省略参数名称

   * 定义注解

     * 使用@interface定义注解

     * 使用元注解@Target定义Annotation可以被应用于源码哪些位置

       Ex. @Target(ElementType.METHOD)

       类或接口： ElementType.TYPE

       字段：ElementType.FIELD

       方法：ElementType.METHOD

       构造方法：ElementType.CONSTRUCTOR

       方法参数：ElementType.PARAMETER

     * 使用元注解@Retention定义Annotation的生命周期：

       仅编译器： RetentionPolicy.SOURCE

       仅class文件：RetentionPolicy.CLASS

       运行期：RetentionPolicy.RUNTIME

       如果@Retention不存在，默认为CLASS

       通常自定义的Annotation都是RUNTIME

     * 可定义多个参数和默认值，参数核心使用value名称

   * 处理注解

     * 可以在运行期通过反射读取RUMTIME类型的注解

       不要漏写@Retention(RetentionPolicy.RUNTIME)

       isAnnotationPresent(Class), getParameterAnnotations()

   

3. ### 泛型 Generics

   * 什么是泛型：泛型就是一种模板，例如ArrayList\<T\>, 在代码中为用到的类创建对应类型，编译器可以针对类型做检查，泛型在继承的时候T不能变（T没有继承关系）

   * 使用泛型可以省略可以自动推断出的类型：ArrayList\<String\> li = new ArrayList<>();

     不指定泛型参数类型时，编译器会给出警告，且只能将\<T\>视为Object类型

   * 编写泛型：

     ~~~java
     public class Pair<T> {
         private T first;
         private T last;
         public Pair(T first, T last){
             this.first = first;
             this.last = last;
         }
         public T getFirst(){
             return first;
         }
         public T getLast(){
             return last;
         }
     }
     ~~~

     * 泛型类型\<T\>不能用于静态方法

     * 泛型也可以同时定义多种类型<T, K>

       public class Pair<T, K>{ ... }

   * 擦拭法 Type Erasure

     * 编译器编译时会把类型\<T\>视为Object，根据\<T\>实现安全的强制转型

     * 擦除导致泛型的局限：

       \<T\>不能是基本类型

       无法取得带泛型的Class，例如：Pair\<String\>.class

       无法判断带泛型的Class，例如： x instanceof Pair\<String\>

       不能直接实例化T类型，例如 new T()，实例化T类型必须借助Class\<T\>

       可能导致方法重名：比如Object的equals方法

     * 子类可以获取父类的泛型类型\<T\>

   * 泛型extends通配符

     * 使用类似<? extends Number>通配符作为方法参数时表示：

       方法内部可以获取调用Number引用的方法：Number n = obj.getXxx()

       方法内部无法调用传入Number引用的方法（null除外）：obj.set(Number n)

     * 使用类似\<T extends Number\> 定义泛型类时表示：泛型类型限定为Number或Number子类

   * 泛型super通配符

     - 使用类似<? super Number>通配符作为方法参数时表示：

       方法内部无法获取调用Integer引用的方法：Integer n = obj.getXxx()

       方法内部可以调用传入Integer引用的方法（null除外）：obj.set(Integer n)

     - 使用类似\<T extends Integer\> 定义泛型类时表示：泛型类型限定为Integer或Integer父类

   * 泛型数组

   

4. ### Java集合 Collections

   * Java集合类定义在java.util包中，包括List, Set, Map等

   * List:
     * ArrayList： 随机读取/修改O(1)，插入/删除O(n)
     * LinkedList：随机读取/修改O(n)，插入/删除O(1)
     * Vector：线程安全的ArrayList ，给所有public方法加上synchronized关键字，性能较低不推荐使用。
     
   * Map:
     * HashMap, 时间复杂度O(1)
     * HashTable: 线程安全的Hashmap，对方法加上了synchronized关键字
     * TreeMap: 基于红黑树(red-black tree)实现，根据键值排序，O(logn)
     * LinkedHashMap: 使用链表实现，保持插入顺序
     * ConcurrentHashMap: HashTable的替代类，采用锁分离实现，将哈希表分成若干Segment，对每个Segment分别加锁
     * 为什么重写equals方法同时要重写hashcode方法：Map/Set执行put/add时调用hashcode类确定当前元素放置位置，如果不重写有可能导致两个逻辑上相同的对象hashcode不同，被重复放入map/set里
     
   * Set
     * HashSet 无序
     * TreeSet/实现SortedSet接口 有序
     
   * Queue 实现了一个先进先出的队列

     实现类LinkedList

     方法offer/peek/poll

     * PriorityQueue出队总取优先级最高的元素出队，需要队内元素实现了Comparable接口或者提供一个Comparator给PriorityQueue构造方法

     * Deque(Double End Queue) 实现类ArrayDeque/LinkedList

       |                  | Queue     | Deque                                     |
       | ---------------- | --------- | ----------------------------------------- |
       | 添加元素到队尾   | add/offer | addLast/offerLast/addFirst/offerFirst     |
       | 取队首元素并删除 | poll      | removeFirst/removeLast/pollFirst/pollLast |
       | 取队首元素不删除 | peek      | getFirst/peekFirst/getLast/peekLast       |

   * Stack 实现了后入先出的数据结构

     ```java
     Deque<Integer> stack = new LinkedList<>();
     ```

     尽量不要使用遗留类Stack（实现Vector）

   * 如何让自己编写的集合类使用for...each循环

     * 实现Iterable接口
     * 返回Iterator对象
     * 用Iterator对象迭代
     
   * 如何编写一个Iterator： 实现hasNext和next方法

       ```java
       class ReadOnlyIterator implements Iterator<E> {
           int index = 0;
           @Override
           public boolean hasNext(){
               return index < ReadOnlyList.this.array.length;
           }
           @Override
           public E next(){
               E e = array[index];
               index++;
               return e;
           }
       }
       ```
       
   * Collections工具类

       * addAll()

       * 创建空集合 emptyList()/emptyMap()/emptySet()

       * 创建单元素集合 singleton(T o)/singletonList(T o)/singletonMap(K key, V value)

       * 排序 sort(List\<T\> list)/sort(List\<T\> list, Comparator c)

       * 随机重置List元素 shuffle(List\<T\> list) （洗牌）

       * 把可变集合变成不可变集合 unmoidifiableList(List<? extends T> list)

       * 把线程不安全的集合变成线程安全的集合synchronizedList(List\<T\> list) (不推荐使用)

         

5. ### Java I/O

    * 字节流byte: InputStream/OutputStream

    字符流char: Reader/Writer

    * 同步I/O：读写代码等待数据返回后才执行后续代码，代码编写简单CPU执行效率低，java.io

    异步I/O：读写代码只发送请求后立刻执行后续代码，代码编写复杂，CPU执行效率高，java.nio

    * java.io: 

    | 抽象类 | InputStream | OutputStream | Reader | Writer |
    | --- | ----- | ----- | ----- | ----- |
    | 实现类 | FileInputStream | FileOutpuStream | FileReader | FileWriter |

    * File:

        * java.io.File表示文件系统的一个文件或目录： `File f = new File("..");`
        * 获取路径getPath(), 获取绝对路径getAbsolutePath(), 获取规范路径(绝对路径的上一层)getCanonicalPath()
        * 判断是否为文件isFile，是否为目录isDirectory()
        * 可以获取目录的文件和子目录
        * 可以通过File对象创建或删除文件和目录

    * InputStream/OutputStream 字节流

        * InputStream定义了所有输入输出流的超类

        * FileInputStream/FileOutputStream是一个实现类，实现了文件流的输入输出

        ```java
        public static void main(String[] args) throws IOException{
            try (InputStream input = new FileInputStream("readme.txt")){
                int n;
                byte[] buffer = new byte[1000];
                while ((n = input.read(buffer) != -1)){
                    System.out.println(n + " bytes read.");
                }
        }
            try (OutputStream output = new FileOutputStream("output.txt")){
                byte[] b1 = "Hello".getBytes("UTF-8");
                output.write(b1);
            }
        }
        ```

        * read/write方法是阻塞的，必须执行完返回后再执行下一行

        * ByteArrayInputStream 从内存字节数组中读取数据，数据源是一个字节数组，继承自InputStream

        * ServletInputStream 从HTTP请求读取数据

        * Socket.getInputStream() 从TCP连接读取数据

        * DataInputStream 数据输入流，继承自FilterInputStream，允许应用程序以与机器无关方式从底层输入流中读写基本Java数据类型

        * BufferedInputStream 带缓冲区的输入流，继承自FilterInputStream,缓冲区默认大小为8M，能够减少访问磁盘的次数，提高文件读取性能

    * Filtler模式/装饰器模式

        * Java IO使用Filter模式为InputStream/OutputStream增加功能

        * 可以把InputStream/OutputStream和任意FilterInputStream/FilterOutputStream组合，可以在运行期动态增加功能

        ```java
        Input input = new GZIPInputStream(
                        new BufferedInputStream(
                        new FileInputStream("test.gz")));
        ```

        * 如何创建一个FilterInputStream类：
        获取一个InputStream类对象，重写对应方法

        ```java
        public class CountInputStream{
            int count = 0;
            public CountInputStream(InputStream in){
                super(in);
            }
        }
        @Override
        public int read(byte[] b, int off, int len) throws IOException{
                int n = super.read(b, off, len);
                count += n;
                return n;
            }
        }
        ```

    * classpath
    从classpath读取文件可以避免文件路径不一致(例如Windows和Linux)

        ```java
        try (InputStream input = getClass().getResourceAsStream("/default.properties")){
            if (input != null){
            // do something
            }
        }
        ```

    * 序列化 Serialize
      
        * 序列化是指把一个Java对象变成二进制内容(byte[]), 可以保存在文件中或通过网络传输
        
        * 要使Java对象能被序列化，需要实现Serializable接口，这个接口没有定义任何方法，是一个标记接口(Marker Interface)
        
        * ObjectOutputStream负责把一个Java对象写入二进制流：
        
        ```java
        try (ObjectOutputStream output = new ObjectOutputStream(...)){
        	output.writeObject(new Person("Tom"));
        }	
        ```
        
        * ObjectInputStream负责从二进制流读取一个Java对象：
          
        ```java
        try (ObjectInputStream input = new ObjectInputStream(...)){
            Object p1 = input.readObject();
            Person p2 = (Person) input.readObject();
        }
        ```

    * 反序列化由JVM直接构造对象，不调用构造方法

    * 可以声明一个serialVersionUID作为版本号，版本号变化时无法反序列化

    * 如果需要与其它语言进行通用序列化，一般使用JSON/XML替代

    * Reader/Writer 字符流

        * Reader用法

          ```java
          public void readFile() throws IOException {
              try (Reader reader = new FileRead("readme.txt")) {
                  char[] buffer = new char[1000];
                  int n;
                  while ((n = reader.read(buffer)) != -1) {
                      System.out.println("read" + n + "chars");
                  }
              }
          }
          ```
        
        
        * Reader实际上时基于InputStream构造的：
        
            ```java
            Reader reader = new InputStreamReader(new FileInputStream("readme.txt"));
            ```






