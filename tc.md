> 整合技术知识
>
> 自己使用
>
> #### 2021



## Java



### 杂货



#### String#equalsIgnoreCase



> ```java
> public boolean equalsIgnoreCase(String anotherString)
> ```



如果给定对象与字符串相等，则返回 true，否则返回 false。











#### treeMap#ceilingEntry() & treeMap#floorEntry()



ceilingEntry(): 获取map中大于等于它且最小的值

floorEntry(): 获取map中小于等于它且最大的值



```java
public class TreeMapDemo {
   public static void main(String[] args) {
      
      // creating tree map 
      NavigableMap<Integer, String> treemap = new TreeMap<Integer, String>();

      // populating tree map
      treemap.put(2, "two");
      treemap.put(1, "one");
      treemap.put(3, "three");
      treemap.put(6, "six");
      treemap.put(5, "five");

      System.out.println("Ceiling entry for 4: "+ treemap.ceilingEntry(4));
      System.out.println("Ceiling entry for 5: "+ treemap.ceilingEntry(5));
   }
}
```

> 得到output：
>
> Ceiling entry for 4: 5=five
> Ceiling entry for 5: 5=five











#### BigDecimal方法

> 个人用到的一些方法：
>
> ```new BigDecimal(String)``` : 如果直接用float转换的话，会有精度问题！
>
> ```divideAndRemainder(BigDecimal) :```
>
> ​	得到BigDecimal[]数组，其中[0]为除后的结果，[1]为余数
>
> ```stripTrailingZeros（）```： 去除尾部的0，用科学计数法表示，可以用
> ```toPlainString()```   把它换成正常的计数[String类型]
>
> ```java
> String res = total.divideAndRemainder(qty)[1].stripTrailingZeros().toPlainString();
> 
> ```
>
> 
>
> ```setScale(int newScale, int roundingMode)```
>
> --去newScale位小数，采用什么模式：
>
> 	* ROUND_DOWN 直接去掉多余的位数
> 	* ROUND_UP 进位
> 	* ROUND_CEILING 正数进位向上，负数舍位向上【-2.22222  ---> -2.22】
> 	* ROUND_FLOOR 正数舍位向下，负数进位向下
> 	* ROUND_HALF_UP 【四舍五入，为0.5时进位】
> 	* ROUND_HALF_DOWN 【四舍五入，大于0.5时才进位】
> 	* ROUND_HALF_EVEN 【如果舍弃部分左边的数字为偶数，作ROUND_HALF_DOWN; 如果舍弃部分左边的数字为技术，则做ROUND_HALF_UP】【4.15---> 4.2;       4.25-----> 4.2】
> 	* ROUND_UNNECESSARY 【断言操作结果是准确的，如果不准确则抛出异常】
>
>  
>
> ```stripTrailingZeros()```
>
> 转为科学计数法，跟```toPlainString()```一起用可以去掉后面的0
>
> ```kotlin
> // data.pieceQty 为 BigDecimal 类型
> data.pieceQty!!.stripTrailingZeros().toPlainString()
> ```
>
> 



















#### System#arrayCopy方法

```java
public static native void arraycopy(Object src,  int  srcPos,
                                    Object dest, int destPos,
                                    int length);
```



- src – the source array.
- srcPos – starting position in the source array.
- dest – the destination array.
- destPos – starting position in the destination data.
- length – the number of array elements to be copied.【复制的长度】





扩展：**Fisher-Yates 洗牌算法** （https://leetcode-cn.com/problems/shuffle-an-array/）

```java
public class ShuffleArray {
    /**
     * Your Solution object will be instantiated and called as such:
     * Solution obj = new Solution(nums);
     * int[] param_1 = obj.reset();
     * int[] param_2 = obj.shuffle();
     */
    // Fisher-Yates 洗牌算法 ---- 其实就是洗个牌吧...
    int[] nums;
    int[] original;

    public ShuffleArray(int[] nums) {
        this.nums = nums;
        this.original = new int[nums.length];
        System.arraycopy(nums, 0, original, 0, nums.length);
    }

    public int[] reset() {
        System.arraycopy(original, 0, nums, 0, nums.length);
        return nums;
    }

    public int[] shuffle() {
        Random random = new Random();
        for (int i = 0; i < nums.length; i++) {
            int j = i + random.nextInt(nums.length - i);
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
        }
        return nums;
    }
}
```
























### 枚举Enum

> 来源：https://github.com/Snailclimb/JavaGuide/blob/master/docs/java/basis/%E7%94%A8%E5%A5%BDJava%E4%B8%AD%E7%9A%84%E6%9E%9A%E4%B8%BE%E7%9C%9F%E7%9A%84%E6%B2%A1%E6%9C%89%E9%82%A3%E4%B9%88%E7%AE%80%E5%8D%95.md
>
> 可以直接看上面的内容，我基本是搬运 = =



#### 1.概览

enum关键字在 java5 中引入，表示一种特殊类型的类，其总是继承java.lang.Enum类，更多内容可以自行查看其[官方文档](https://docs.oracle.com/javase/6/docs/api/java/lang/Enum.html)。

**以这种方式定义的常量使代码更具可读性，允许进行编译时检查，预先记录可接受值的列表，并避免由于传入无效值而引起的意外行为。**

下面示例定义一个简单的枚举类型 pizza 订单的状态，共有三种 ORDERED, READY, DELIVERED状态:

```java
package shuang.kou.enumdemo.enumtest;

public enum PizzaStatus {
    ORDERED,
    READY, 
    DELIVERED; 
}
```

**简单来说，我们通过上面的代码避免了定义常量，我们将所有和 pizza 订单的状态的常量都统一放到了一个枚举类型里面。**

```java
System.out.println(PizzaStatus.ORDERED.name());//ORDERED
System.out.println(PizzaStatus.ORDERED);//ORDERED
System.out.println(PizzaStatus.ORDERED.name().getClass());//class java.lang.String
System.out.println(PizzaStatus.ORDERED.getClass());//class shuang.kou.enumdemo.enumtest.PizzaStatus
```



#### 2.自定义枚举方法

现在我们对枚举是什么以及如何使用它们有了基本的了解，让我们通过在枚举上定义一些额外的API方法，将上一个示例提升到一个新的水平：

```java
public class Pizza {
    private PizzaStatus status;
    public enum PizzaStatus {
        ORDERED,
        READY,
        DELIVERED;
    }
 
    public boolean isDeliverable() {
        return getStatus() == PizzaStatus.READY;
    }
     
    // Methods that set and get the status variable.
}
```





#### 3.使用 == 比较枚举类型

> todo/......

#### 4. 在switch语句中使用枚举类型












## Java规范

> 来源：
>
> 《Java开发手册》v1.5.0 华山版.pdf
>
> https://edu.aliyun.com/course/417?spm=5176.10731460.0.0.3f8e53b15TcUms















值得注意的准则：

### 集合：

> 1. 使用 Map 的方法 keySet()/values()/entrySet()返回集合对象时，不可以对其进行添 加元素操作，否则会抛出 UnsupportedOperationException 异常。
>
> 
>
> 2. 在 subList 场景中，高度注意对原集合元素的增加或删除，均会导致子列表的遍 历、增加、删除产生 ConcurrentModificationException 异常。
>
> 
>
> 3. 【强制】使用集合转数组的方法，必须使用集合的 toArray(T[] array)，传入的是类型完全一 致、长度为 0 的空数组。 反例：直接使用 toArray 无参方法存在问题，此方法返回值只能是 Object[]类，若强转其它类型数组将出 现 ClassCastException 错误。 
>
>    正例： 
>
>    ```java
>    List list = new ArrayList<>(2); 
>    list.add("guan"); 
>    list.add("bao"); 
>    String[] array = list.toArray(new String[0]); 
>    ```
>
>    说明：使用 toArray 带参方法，数组空间大小的 length：
>
>     1） 等于 0，动态创建与 size 相同的数组，性能最好。
>
>     2） 大于 0 但小于 size，重新创建大小等于 size 的数组，增加 GC 负担。 Java 开发手册 12/44 
>
>    3） 等于 size，在高并发情况下，数组创建完成之后，size 正在变大的情况下，负面影响与上相同。 
>
>    4） 大于 size，空间浪费，且在 size 处插入 null 值，存在 NPE 隐患。
>
> 
>
> 4.  【强制】使用工具类 Arrays.asList()把数组转换成集合时，不能使用其修改集合相关的方 法，它的 add/remove/clear 方法会抛出 UnsupportedOperationException 异常。
>
>     说明：asList 的返回对象是一个 Arrays 内部类，并没有实现集合的修改方法。Arrays.asList 体现的是适 配器模式，只是转换接口，后台的数据仍是数组。 
>
>    ```java
>    String[] str = new String[] { "yang", "hao" };
>    List list = Arrays.asList(str); 
>    ```
>
>    第一种情况：list.add("yangguanbao"); 运行时异常。
>
>     第二种情况：str[0] = "changed"; 也会随之修改，反之亦然。
>
> 
>
> 5. 【推荐】使用 entrySet 遍历 Map 类集合 KV，而不是 keySet 方式进行遍历。 
>
>    说明：keySet 其实是遍历了 2 次，一次是转为 Iterator 对象，另一次是从 hashMap 中取出 key 所对应 的 value。而 entrySet 只是遍历了一次就把 key 和 value 都放到了 entry 中，效率更高。如果是 JDK8， 使用 Map.forEach 方法。
>
> 
>
> 6. 【推荐】高度注意 Map 类集合 K/V 能不能存储 null 值的情况，如下表格： 集合类 Key Value Super 说明 Hashtable 不允许为 null 不允许为 null Dictionary 线程安全 ConcurrentHashMap 不允许为 null 不允许为 null AbstractMap 锁分段技术（JDK8:CAS） TreeMap 不允许为 null 允许为 null AbstractMap 线程不安全 HashMap 允许为 null 允许为 null AbstractMap 线程不安全 反例：由于 HashMap 的干扰，很多人认为 ConcurrentHashMap 是可以置入 null 值，而事实上，存储 null 值时会抛出 NPE 异常。
>
> 
>
> 7.  【强制】在使用 Collection 接口任何实现类的 addAll()方法时，都要对输入的集合参数进行 NPE 判断。
>
>     说明：在 ArrayList#addAll 方法的第一行代码即 Object[] a = c.toArray(); 其中 c 为输入集合参数，如果 为 null，则直接抛出异常。
>
> 8. 【推荐】高度注意 Map 类集合 K/V 能不能存储 null 值的情况，如下表格：
>  
>  | 集合类            | Key           | Value         | Super       | 说明                  |
>  | ----------------- | ------------- | ------------- | ----------- | --------------------- |
>  | Hashtable         | 不允许为 null | 不允许为 null | Dictionary  | 线程安全              |
>  | ConcurrentHashMap | 不允许为null  | 不允许为null  | AbstractMap | 锁分段技术（JDK8:CAS) |
>  | TreeMap           | 不允许为null  | 允许为null    | AbstractMap | 线程不安全            |
>  | HashMap           | 允许为null    | 允许为null    | AbstractMap | 线程不安全            |
>  
>  
>
> 
>
>     Hashtable 不允许为 null 不允许为 null Dictionary 线程安全 ConcurrentHashMap 不允许为 null 不允许为 null AbstractMap 锁分段技术（JDK8:CAS） TreeMap 不允许为 null 允许为 null AbstractMap 线程不安全 HashMap 允许为 null 允许为 null AbstractMap 线程不安全 
>
> 9. 反例：由于 HashMap 的干扰，很多人认为 ConcurrentHashMap 是可以置入 null 值，而事实上，存储 null 值时会抛出 NPE 异常。



### 并发处理：

1. 【强制】创建线程或线程池时请指定有意义的线程名称，方便出错时回溯。 

   正例：自定义线程工厂，并且根据外部特征进行分组，比如机房信息。 

   ```java
   public class UserThreadFactory implements ThreadFactory { 
       private final String namePrefix; 
       private final AtomicInteger nextId = new AtomicInteger(1);
       
       // 定义线程组名称，在 jstack 问题排查时，非常有帮助 
       UserThreadFactory(String whatFeaturOfGroup) {
           namePrefix = "From UserThreadFactory's " + whatFeaturOfGroup + "-Worker-";
       } 
       
       @Override 
       public Thread newThread(Runnable task) { 
           String name = namePrefix + nextId.getAndIncrement();
           Thread thread = new Thread(null, task, name, 0, false);
           System.out.println(thread.getName()); 
           return thread; 
       } 
   }
   ```

2. 【强制】线程池不允许使用 Executors 去创建，而是通过 ThreadPoolExecutor 的方式，这 样的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险。 

   说明：Executors 返回的线程池对象的弊端如下： 

   1） FixedThreadPool 和 SingleThreadPool： 允许的请求队列长度为 Integer.MAX_VALUE，可能会堆积大量的请求，从而导致 OOM。 

   2） CachedThreadPool： 允许的创建线程数量为 Integer.MAX_VALUE，可能会创建大量的线程，从而导致 OOM。

   

3.  【强制】SimpleDateFormat 是线程不安全的类，一般不要定义为 static 变量，如果定义为 static，必须加锁，或者使用 DateUtils 工具类。

    正例：注意线程安全，使用 DateUtils。亦推荐如下处理： 

   ```java
   private static final ThreadLocal df = new ThreadLocal() {
       @Override  protected DateFormat initialValue() {  
           return new SimpleDateFormat("yyyy-MM-dd");  
       }  
   };
   ```

   说明：如果是 JDK8 的应用，可以使用 Instant 代替 Date，LocalDateTime 代替 Calendar， DateTimeFormatter 代替 SimpleDateFormat，官方给出的解释：simple beautiful strong immutable  thread-safe。

   

4. 【强制】对多个资源、数据库表、对象同时加锁时，需要保持一致的加锁顺序，否则可能会 造成死锁。

    说明：线程一需要对表 A、B、C 依次全部加锁后才可以进行更新操作，那么线程二的加锁顺序也必须是 A、B、C，否则可能出现死锁。

   

5. 【强制】在使用阻塞等待获取锁的方式中，必须在 try 代码块之外，并且在加锁方法与 try 代 码块之间没有任何可能抛出异常的方法调用，避免加锁成功后，在 finally 中无法解锁。

    说明一：如果在 lock 方法与 try 代码块之间的方法调用抛出异常，那么无法解锁，造成其它线程无法成功 获取锁。 

   说明二：如果 lock 方法在 try 代码块之内，可能由于其它方法抛出异常，导致在 finally 代码块中， unlock 对未加锁的对象解锁，它会调用 AQS 的 tryRelease 方法（取决于具体实现类），抛出 IllegalMonitorStateException 异常。 

   说明三：在 Lock 对象的 lock 方法实现中可能抛出 unchecked 异常，产生的后果与说明二相同。 

   正例：

   ```java
   Lock lock = new XxxLock(); 
   //...
   lock.lock();
   try {
       doSomething();
       doOthers();
   } finally {
       lock.unlock();
   }
   ```

    反例： 

   ```java
   Lock lock = new XxxLock();
   //...
   try {
        // 如果此处抛出异常，则直接执行 finally 代码块
       doSomething();
       //无论加锁是否成功，finally 代码块都会执行
       lock.lock();
       doOthers();
   } finally {
       lock.unlock();
   }
   ```

   

6. 【强制】在使用尝试机制来获取锁的方式中，进入业务代码块之前，必须先判断当前线程是 否持有锁。锁的释放规则与锁的阻塞等待方式相同。 

   说明：Lock 对象的 unlock 方法在执行时，它会调用 AQS 的 tryRelease 方法（取决于具体实现类），如果 当前线程不持有锁，则抛出 IllegalMonitorStateException 异常。

   ```java
   Lock lock = new XxxLock();
   // ...
   boolean isLocked = lock.tryLock();
   if (isLocked) {
       try {
           doSomething();
           doOthers();
       } finally {
           lock.unlock();
       }
   }
   ```

   

### 控制语句

1.  【强制】在高并发场景中，避免使用”等于”判断作为中断或退出的条件。 

   说明：如果并发控制没有处理好，容易产生等值判断被“击穿”的情况，使用大于或小于的区间判断条件 来代替。 

   反例：判断剩余奖品数量等于 0 时，终止发放奖品，但因为并发处理错误导致奖品数量瞬间变成了负数， 这样的话，活动无法终止。

2. 【参考】特殊注释标记，请注明标记人与标记时间。注意及时处理这些标记，通过标记扫 描，经常清理此类标记。线上故障有时候就是来源于这些标记处的代码。 

   1） 待办事宜（**TODO**）:（标记人，标记时间，[预计处理时间]） 表示需要实现，但目前还未实现的功能。这实际上是一个 Javadoc 的标签，目前的 Javadoc 还没 有实现，但已经被广泛使用。只能应用于类，接口和方法（因为它是一个 Javadoc 标签）。 

   2） 错误，不能工作（**FIXME**）:（标记人，标记时间，[预计处理时间]） 在注释中用 FIXME 标记某代码是错误的，而且不能工作，需要及时纠正的情况。





### 异常

1. 【强制】不要在 finally 块中使用 return。

    说明：try 块中的 return 语句执行成功后，并不马上返回，而是继续执行 finally 块中的语句，如果此处存 在 return 语句，则在此直接返回，无情丢弃掉 try 块中的返回点。

    反例：

   ```java
   private int x = 0;
   publi int checkReturn() {
       try {
           //x等于1，此处不返回
           return ++x;
       } finally {
           //返回的结果是2
           return ++x;
       }
   }
   ```



### 日志

> ps.到时补充



### MySQL

1. 【参考】合适的字符存储长度，不但节约数据库表空间、节约索引存储，更重要的是提升检 索速度。 正例：如下表，其中无符号值可以避免误存负数，且扩大了表示范围。 

   | 对象     | 年龄区间   | 类型              | 字节 | 表示范围                       |
   | -------- | ---------- | ----------------- | ---- | ------------------------------ |
   | 人       | 150 岁之内 | tinyint unsigned  | 1    | 无符号值：0 到 255             |
   | 龟       | 数百岁     | smallint unsigned | 2    | 无符号值：0 到 65535           |
   | 恐龙化石 | 数千万年   | int unsigned      | 4    | 无符号值：0 到约 42.9 亿       |
   | 太阳     | 约 50 亿年 | bigint unsigned   | 8    | 无符号值：0 到约 10 的 19 次方 |

2.  【强制】业务上具有唯一特性的字段，即使是多个字段的组合，也必须建成唯一索引。 说明：不要以为唯一索引影响了 insert 速度，这个速度损耗可以忽略，但提高查找速度是明显的；另外， 即使在应用层做了非常完善的校验控制，只要没有唯一索引，根据墨菲定律，必然有脏数据产生。

   

3.  【强制】超过三个表禁止 join。需要 join 的字段，数据类型必须绝对一致；多表关联查询 时，保证被关联的字段需要有索引。 说明：即使双表 join 也要注意表索引、SQL 性能。

   

4.  【强制】在 varchar 字段上建立索引时，必须指定索引长度，没必要对全字段建立索引，根据 实际文本区分度决定索引长度即可。

   说明：索引的长度与区分度是一对矛盾体，一般对字符串类型数据，长度为 20 的索引，区分度会高达 90%以上，可以使用 count(distinct left(列名, 索引长度))/count(*)的区分度来确定。

   

5. 【推荐】如果有 order by 的场景，请注意利用索引的有序性。order by 最后的字段是组合 索引的一部分，并且放在索引组合顺序的最后，避免出现 file_sort 的情况，影响查询性能。 

   正例：where a=? and b=? order by c; 索引：a_b_c 

   反例：索引如果存在范围查询，那么索引有序性无法利用，如：WHERE a>10 ORDER BY b; 索引 a_b 无 法排序。

   

6.  【强制】不要使用 count(列名)或 count(常量)来替代 count(*)，count(*)是 SQL92 定义的 标准统计行数的语法，跟数据库无关，跟 NULL 和非 NULL 无关。 说明：count(*)会统计值为 NULL 的行，而 count(列名)不会统计此列为 NULL 值的行。

   

7.  【强制】count(distinct col) 计算该列除 NULL 之外的不重复行数，注意 count(distinct  col1, col2) 如果其中一列全为 NULL，那么即使另一列有不同的值，也返回为 0。

   

8.  【强制】当某一列的值全是 NULL 时，count(col)的返回结果为 0，但 sum(col)的返回结果 为 NULL，因此使用 sum()时需注意 NPE 问题。 

   正例：使用如下方式来避免 sum 的 NPE 问题：SELECT IFNULL(SUM(column), 0) FROM table;

   

9.  【强制】禁止使用存储过程，存储过程难以调试和扩展，更没有移植性。

   

10.  【强制】数据订正（特别是删除、修改记录操作）时，要先 select，避免出现误删除，确认无 误才能执行更新语句。





















## CRON表达式

> 说明： 项目中有用到，稍微看了一下
>
> 参考文档：https://help.aliyun.com/knowledge_detail/64769.html



Cron表达式是一个字符串，字符串以5或6个空格隔开，分为6或7个域，每一个域代表一个含义，Cron有如下两种语法格式：

　　（1） Seconds Minutes Hours DayofMonth Month DayofWeek Year

　　（2）*Seconds Minutes Hours DayofMonth Month DayofWeek*



corn从左到右（用空格隔开）：秒 分 小时 月份中的日期 月份 星期中的日期 年份



| 字段                     | 允许值                                 | 允许的特殊字符             |
| ------------------------ | -------------------------------------- | -------------------------- |
| 秒（Seconds）            | 0~59的整数                             | , - * /   四个字符         |
| 分（*Minutes*）          | 0~59的整数                             | , - * /   四个字符         |
| 小时（*Hours*）          | 0~23的整数                             | , - * /   四个字符         |
| 日期（*DayofMonth*）     | 1~31的整数（但是你需要考虑你月的天数） | ,- * ? / L W C   八个字符  |
| 月份（*Month*）          | 1~12的整数或者 JAN-DEC                 | , - * /   四个字符         |
| 星期（*DayofWeek*）      | 1~7的整数或者 SUN-SAT （1=SUN）        | , - * ? / L C #   八个字符 |
| 年(可选，留空)（*Year*） | 1970~2099                              | , - * /   四个字符         |

 

**注意事项:**

（1）*：表示匹配该域的任意值。假如在Minutes域使用*, 即表示每分钟都会触发事件。

　　（2）?：只能用在DayofMonth和DayofWeek两个域。它也匹配域的任意值，但实际不会。因为DayofMonth和DayofWeek会相互影响。例如想在每月的20日触发调度，不管20日到底是星期几，则只能使用如下写法： 13 13 15 20 * ?, 其中最后一位只能用？，而不能使用*，如果使用*表示不管星期几都会触发，实际上并不是这样。

　　（3）-：表示范围。例如在Minutes域使用5-20，表示从5分到20分钟每分钟触发一次 

　　（4）/：表示起始时间开始触发，然后每隔固定时间触发一次。例如在Minutes域使用5/20,则意味着5分钟触发一次，而25，45等分别触发一次. 

　　（5）,：表示列出枚举值。例如：在Minutes域使用5,20，则意味着在5和20分每分钟触发一次。 

　　（6）L：表示最后，只能出现在DayofWeek和DayofMonth域。如果在DayofWeek域使用5L,意味着在最后的一个星期四触发。 

　　（7）W:表示有效工作日(周一到周五),只能出现在DayofMonth域，系统将在离指定日期的最近的有效工作日触发事件。例如：在 DayofMonth使用5W，如果5日是星期六，则将在最近的工作日：星期五，即4日触发。如果5日是星期天，则在6日(周一)触发；如果5日在星期一到星期五中的一天，则就在5日触发。另外一点，W的最近寻找不会跨过月份 。

　　（8）LW:这两个字符可以连用，表示在某个月最后一个工作日，即最后一个星期五。 

　　（9）#:用于确定每个月第几个星期几，只能出现在DayofMonth域。例如在4#2，表示某月的第二个星期三。











## Kotlin语法

> https://kotlinlang.org/docs/home.html

### Basics



#### Program entry point

```kotlin
fun main() {
    printlin("Hello world")
}
```



Another form of ```main``` accepts a variable number of ```String``` arguments.

```kotlin
fun main(args: Array<String>) {
    printlin(args.contentToString())
}
```













#### Basic types

> Basic types used in Kotlin: **numbers, booleans, characters, strings and arrays**.



##### Numbers

| Type  | Size (bits) | Min value                          | Max value                           |
| ----- | ----------- | ---------------------------------- | ----------------------------------- |
| Byte  | 8           | -128                               | 127                                 |
| Short | 16          | -32768                             | 32767                               |
| Int   | 32          | -2,147,483,648 (-2 31)             | 2,147,483,647 (2 31- 1)             |
| Long  | 64          | -9,223,372,036,854,775,808 (-2 63) | 9,223,372,036,854,775,807 (2 63- 1) |

类型自动推断：

```kotlin
val one = 1 // Int
val threeBillion = 3000000000 // Long
val oneLong = 1L // Long
val oneByte: Byte = 1
```





**Variables**

Read-only local variables are defined using the keyword ```val```.

They can be assigned a value only once.

```kotlin
val a: Int = 1
val b = 2
val c: Int
c = 3
```



>  Variables that can be reassigned us the ```var``` keyword.







**Classes and instances**

> To define a class, use the ```class``` keyword.
>
> ```class Shape```
>
> Properties of a class can be listed in its declaration or body.
>
> ```kotlin
> class Rectangle(var height: Double, var length: Double) {
>     var perimeter = (height +length) * 2;
> }
> 
> 
> val rectangle = Rectangle(5.0, 2.0);
> println("The perimeter is ${rectangle.perimeter}")
> ```
>
>  
>
> Inheritance(继承) between classes is declared by colon(```:```). Classes are final by default; to make a class inheritable, mark it as `open`.
>
> ```kotlin
> open class Shape
> 
> class Rectangle(var height: Double, var length: Double): Shape() {
>     var perimeter = (height +length) * 2;
> }
> ```
>
> 





**String template**

```kot
var a = 1
// simple name in template:
val s1 = "a is $a" 

a = 2
// arbitrary expression in template:
val s2 = "${s1.replace("is", "was")}, but now is $a"
```





**Conditional expressions**

```kot
fun maxOf(a: Int, b: Int) = if(a > b) a else b
```







**for loop**

```kotlin
val items = listOf("apple", "banana")
for(item in items) {
    println(item)
}


// or

val items = listOf("apple", "banana", "kiwifruit")
for (index in items.indices) {
    println("item at $index is ${items[index]}")
}
```





**When expression**

```kotlin
fun describe(obj: Any): String = 
	when (obj) {
        1 -> "One"
        "Hello" -> "Greeting"
        is Long -> "Long"
        !is String -> "Not a string"
        else -> "unkown"
    }
```





**Collections**

Check if a collection contains an object using `in` operator.

```kotlin
when {
    "orange" in items -> println("juicy")
    "apple" in items -> println("apple is fine too")
}
```



Using lambda expressions to filter and map collections:

```kotlin
val fruits = listOf("banana", "avocado", "apple", "kiwifruit")
fruits
    .filter { it.startsWith("a") }
    .sortedBy { it }
    .map { it.uppercase() }
    .forEach { println(it) }
```









**Nullable values and null check**

Return `null` if `str` does not hold an integer:

```kotlin
fun parseInt(str: String): Int? {
    // ...
}
```







#### Collections

Map:

```kotlin
//固定的不可变的map
val map = mapOf<String, String>("中国" to "China", "英国" to "England")

//可变的map
private val countMap = mutableMapOf<String, Int>("case" to 0, "box" to 0, "bottle" to 0)

//java中的map(就底层是java)
val map3 = Hashtable<String, Int>()
map["111"] = 111

//遍历
map.forEach { key, value -> println(key + ":" + value) }

for((key: String, value: String) in map) {
    println(key + ":" + value)
}

//遍历key
val keySet = map.keys
keySet.forEach {println(it)}

//遍历value
val values = map.values
values.forEach { println(it) }
```







List:

```kotlin
//不可变list
val aList: List<Int> = listOf()
val bList = List<Int> = listOf(1, 3, 5, 7)
println(bList)

//可变list
val cList = mutableListOf(2, 4, 5)
cList.add(0, 0) //在下标0插入
//或者用arrayListOf
val mList = arrayListOf("x1", "x2", "x3")

//转为可变的mutableList
val dList = bList.toMutableList()

//交集
dList.retainAll(cList)
println(dList) //[5]

//any /all
println(cList.any{it == 2}) //true
println(cList.all{it % 2 == 0}) //false

//查看集合有没有元素,如果集合中没有元素，则返回true，否则返回false。
println(dList.none()) //false

//none函数
println(dList.none{it == 4}) //true

//返回个数
println(dList.count())
println(dList.count{it == 5}) //返回1

//map函数
prinln(cList.map {it + 1}) //3,5,6

//mapNotNull:遍历集合每个元素，得到通过函数算子transform映射之后的值，剔除掉这些值中的null，返回一个无null元素的集合
val eList = mutableListOf(1, null, 2, null, 3)
println(eList.mapNotNull{it}) //1,2,3

//sort() reversed()排序
println(eList.sorted())
println(eList.sortedDescending())


//合并两个list,用plus()
println(dList.plus(eList))
```





























### 类

#### 泛型

与 Java 一样，Kotlin 也提供泛型，为类型安全提供保证，消除类型强转的烦恼。

声明一个泛型类:

```kotlin
class Box<T>(t: T) {
    var value = t
}
```

创建类的实例时我们需要指定类型参数:

```kotlin
val box: Box<Int> = Box<Int>(1)
// 或者
val box = Box(1) // 编译器会进行类型推断，1 类型 Int，所以编译器知道我们说的是 Box<Int>。
```









#### 继承

Kotlin 中所有类都继承该 Any 类，它是所有类的超类，对于没有超类型声明的类是默认超类：

```kotlin
class Example // 从 Any 隐式继承
```

Any 默认提供了三个函数：

```kotlin
equals()

hashCode()

toString()
```

注意：Any 不是 java.lang.Object。

如果一个类要被继承，可以使用 open 关键字进行修饰。

```kotlin
open class Base(p: Int)           // 定义基类

class Derived(p: Int) : Base(p)
```







> 如果子类有主构造函数， 则基类必须在主构造函数中立即初始化。
>
> ```kotlin
> open class Person(var name : String, var age : Int){// 基类
> 
> }
> 
> class Student(name : String, age : Int, var no : String, var score : Int) : Person(name, age) {
> 
> }
> ```
>
>  
>
> 如果子类没有主构造函数，则必须在每一个二级构造函数中用 super 关键字初始化基类，或者在代理另一个构造函数。初始化基类时，可以调用基类的不同构造方法。
>
> ```kotlin
> class Student : Person {
> 
>     constructor(ctx: Context) : super(ctx) {
>         
>     } 
> 
>     constructor(ctx: Context, attrs: AttributeSet) : super(ctx,attrs) {
>         
>     }
> }
> ```







**重写**

在基类中，使用fun声明函数时，此函数默认为final修饰，不能被子类重写。如果允许子类重写该函数，那么就要手动添加 open 修饰它, 子类重写方法使用 override 关键词：

```kotlin
/**用户基类**/
open class Person{
    open fun study(){       // 允许子类重写
        println("我毕业了")
    }
}

/**子类继承 Person 类**/
class Student : Person() {

    override fun study(){    // 重写方法
        println("我在读大学")
    }
}

fun main(args: Array<String>) {
    val s =  Student()
    s.study();

}
```











## Groovy语法

> 补充语法：





---判断字符串在不在数组里面，可以用in：

```groovy
if(sh.sourcePlatform.toUpperCase() in ['PDD', 'PINDUODUO', 'DOUYIN']) {
    //...
}
```



















## 位运算

> Java提供的位运算符有： 左移(<<), 右移(>>), 无符号右移(>>>), 位与(&), 位或(|), 位非(~), 位异或(^), 除了~为一元运算符，其它都为二元运算符。
>
> 参考：https://blog.csdn.net/xiaochunyong/article/details/7748713

1. 左移(<<)

   ```5<<2```

   首先会将5转为2进制表示形式(java中，整数默认就是int类型,也就是32位):

   0000 0000 0000 0000 0000 0000 0000 0101           然后左移2位后，低位补0：

   0000 0000 0000 0000 0000 0000 0001 0100           换算成10进制为20

   

2. 右移(>>)

   还是先将5转为2进制表示形式：

   0000 0000 0000 0000 0000 0000 0000 0101 然后右移2位，高位补0：

   0000 0000 0000 0000 0000 0000 0000 0001

   

3. 无符号右移(>>>)

   ```java
   System.out.println(5>>3);	//0
   System.out.println(-5>>3);   //-1
   System.out.println(-5>>>3)    //536870911
   ```

    -5换算成二进制： 1111 1111 1111 1111 1111 1111 1111 1011 【-x = !x + 1, 补码 = 反码 + 1】

   -5右移3位后结果为-1，-1的二进制为：  1111 1111 1111 1111 1111 1111 1111 1111   // (用1进行补位)

   -5无符号右移3位后的结果 536870911 换算成二进制：  0001 1111 1111 1111 1111 1111 1111 1111   // (用0进行补位)

   > 正数右移，高位用0补，负数右移，高位用1补，负数无符号右移，高位按0补
   >
   > 左移，则都是用0补

   

4. 位与(&)

   ```5 & 3 = 1```

   5转换为二进制：0000 0000 0000 0000 0000 0000 0000 0101

   3转换为二进制：0000 0000 0000 0000 0000 0000 0000 0011

   -------------------------------------------------------------------------------------

   1转换为二进制：0000 0000 0000 0000 0000 0000 0000 0001

   位与：**第一个操作数的的第n位于第二个操作数的第n位如果都是1，那么结果的第n为也为1，否则为0**

   

5. 位或(|)

   ```5 | 3 = 7```

   5转换为二进制：0000 0000 0000 0000 0000 0000 0000 0101

   3转换为二进制：0000 0000 0000 0000 0000 0000 0000 0011

   -------------------------------------------------------------------------------------

   7转换为二进制：0000 0000 0000 0000 0000 0000 0000 0111

   位或操作：**第一个操作数的的第n位于第二个操作数的第n位 只要有一个是1，那么结果的第n为也为1，否则为0**

   

6. 位异或(^)

   ```5 ^ 3 = 6```

   5转换为二进制：0000 0000 0000 0000 0000 0000 0000 0101

   3转换为二进制：0000 0000 0000 0000 0000 0000 0000 0011

   -------------------------------------------------------------------------------------

   6转换为二进制：0000 0000 0000 0000 0000 0000 0000 0110

   位异或：**第一个操作数的的第n位于第二个操作数的第n位 相反，那么结果的第n为也为1，否则为0**

   

7. 位非(~)

   ```~5 = -6```

​    5转换为二进制：0000 0000 0000 0000 0000 0000 0000 0101

   -6转换为二进制：1111 1111 1111 1111 1111 1111 1111 1010

   位非：**操作数的第n位为1，那么结果的第n位为0，反之**





补充：**负数算二进制**

> 比如-5
>
> 把5转为二进制：
>
> 0 0 0 0 0 1 0 1
>
> 取反码：
>
> 1 1 1 1 1 0 1 0
>
> +1取补码：
>
> 1 1 1 1 1 0 1 1（-5）
>
> ---
>
> 求一个二进制数：
>
> 1 1 1 1 0 0 1 1
>
> 先减1：
>
> 1 1 1 1 0 0 1 0
>
> 取反：
>
> 0 0 0 0 1 1 0 1 （13）





#### 利用二进制做加法运算：

LeetCode题：https://leetcode-cn.com/problems/sum-of-two-integers/

比如： 求879 + 346

 0 0 0 0 0 0 1 1 0 1 1 0 1 1 1 1     a(1)--512+256+64+32+8+4+2+1=879

 0 0 0 0 0 0 0 1 0 1 0 1 1 0 1 0     b(2)--256+64+16+8+2=346

___

 0 0 0 0 0 0 0 1 0 1 0 0 1 0 1 0	(& 运算后)  

 0 0 0 0 0 0 1 0 1 0 0 1 0 1 0 0     (左移一位) ==== b(4)

 0 0 0 0 0 0 1 0 0 0 1 1 0 1 0 1     (1与 2 进行 ^运算后) ====== a(3)

----------------------------------

 0 0 0 0 0 0 1 0 0 0 0 1 0 1 0 0    (& 运算后)  

 0 0 0 0 0 1 0 0 0 0 1 0 1 0 0 0     (左移一位) ==== b(6)

 0 0 0 0 0 0 0 0 1 0 1 0 0 0 0 1     (3与 4 进行 ^运算后) ====== a(5)

---

 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0    (& 运算后)  

 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0    (左移一位) ==== b(8)

 0 0 0 0 0 1 0 0 1 0 0 0 1 0 0 1    (5与 6 进行 ^运算后) ====== a(7)

---

0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0    (& 运算后)  

0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0    (左移一位)  ==== b(10)

0 0 0 0 0 1 0 0 1 1 0 0 1 0 0 1    (7与 8 进行 ^运算后) ====== a(9)

---

return a(9) --- 1024+128+64+8+1 = 1225







#### 注意：

**& 运算的优先级比 ^ 高！！**

```java
boolean countDiff = true;
boolean mDiff = false;

int tip = countDiff & mDiff ? 1 : 2;

tip = countDiff ^ mDiff & mDiff ? 3 : tip;       // 3

int tip2 = (countDiff ^ mDiff) & mDiff ? 3 : tip;     //tip
```









#### 运算符优先级

| 优先级 |                           运算符                            |  综合性  |
| :----: | :---------------------------------------------------------: | :------: |
|   1    |                           () [] .                           | 从左往右 |
|   2    |                 ! +(正)   -(负)   ~  ++  --                 | 从右往左 |
|   3    |                          *   /   %                          | 从左往右 |
|   4    |                        +(加)   -(减)                        | 从左往右 |
|   5    |                        <<   >>   >>>                        | 从左往右 |
|   6    |             <       <=    >   >=    instanceof              | 从左往右 |
|   7    |                           ==   !=                           | 从左往右 |
|   8    |                          &(按位与)                          | 从左往右 |
|   9    |                              ^                              | 从左往右 |
|   10   |                             \|                              | 从左往右 |
|   11   |                             &&                              | 从左往右 |
|   12   |                            \|\|                             | 从左往右 |
|   13   |                             ?:                              | 从右往左 |
|   14   | =     +=   -=  *=  %= /=  &=  \|=  ^= ~=   <<=   >>=   >>>= | 从右往左 |











#### 数字的补数

> 给你一个 正 整数 num ，输出它的补数。补数是对该数的二进制表示取反。
>
>  
>
> 示例 1：
>
> 输入：num = 5
> 输出：2
> 解释：5 的二进制表示为 101（没有前导零位），其补数为 010。所以你需要输出 2 。
> 示例 2：
>
> 输入：num = 1
> 输出：0
> 解释：1 的二进制表示为 1（没有前导零位），其补数为 0。所以你需要输出 0 。
>
>
> 提示：
>
> * 给定的整数 num 保证在 32 位带符号整数的范围内。
> * num >= 1
> * 你可以假定二进制数不包含前导零位。
> * 本题与 1009 https://leetcode-cn.com/problems/complement-of-base-10-integer/ 相同
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/number-complement



```java
class Solution {
    public int findComplement(int num) {
        int temp = 0;
        for(int i = 1; i <= 30; i++) {
            if(num >= 1 << i) {
                temp = i;
            } else {
                break;
            }
        }
        //当 i=30i=30 时，构造 \textit{mask} = 2^{i+1} - 1mask=2i + 1 −1 的过程中需要保证不会产生整数溢出
        //注意：16进制为0x开头（阿拉伯数字0），不是Ox
        int t = temp == 30 ? 0x7fffffff : (1 << (temp + 1)) - 1;
        return num ^ t;
    }
}
```

































## Java命令

#### 1. JPS

>jdk中的jps命令可以显示当前运行的java进程以及相关参数，它的实现机制如下：
>java程序在启动以后，会在`java.io.tmpdir`指定的目录下，就是临时文件夹里，生成一个类似于`hsperfdata_User`的文件夹，这个文件夹里（在Linux中为/tmp/hsperfdata_{userName}/），有几个文件，名字就是java进程的pid，因此列出当前运行的java进程，只是把这个目录里的文件名列一下而已。 至于系统的参数什么，就可以解析这几个文件获得。



**命令**

> * jps 
>
> * jps -q
>
>   只显示pid,不显示calss名称，jar文件名和传递给main方法的参数
>
> * jps -l
>
>   输出应用程序main class 的完整package名，或者应用程序jar的完整路径名
>
> * jps -v
>
>   输出传递给JVM的参数
>
> * jps -m
>
>   输出传递给main 方法的参数，在嵌入式jvm上可能是null





#### 2. Jstack

> jstack用于生成java虚拟机当前时刻的线程快照。线程快照是当前java虚拟机内每一条线程正在执行的方法堆栈的集合，生成线程快照的主要目的是定位线程出现长时间停顿的原因，如线程间死锁、死循环、请求外部资源导致的长时间等待等。 线程出现停顿的时候通过jstack来查看各个线程的调用堆栈，就可以知道没有响应的线程到底在后台做什么事情，或者等待什么资源。 如果java程序崩溃生成core文件，jstack工具可以用来获得core文件的java stack和native stack的信息，从而可以轻松地知道java程序是如何崩溃和在程序何处发生问题。另外，jstack工具还可以附属到正在运行的java程序中，看到当时运行的java程序的java stack和native stack的信息, 如果现在运行的java程序呈现hung的状态，jstack是非常有用的。
>
> 
>
> > **jstack命令主要用来查看Java线程的调用堆栈的，可以用来分析线程问题（如死锁）。**
>
> 
>
> **线程状态**
>
> 想要通过jstack命令来分析线程的情况的话，首先要知道线程都有哪些状态，下面这些状态是我们使用jstack命令查看线程堆栈信息时可能会看到的**线程的几种状态**：
>
> > NEW,未启动的。不会出现在Dump中。
> >
> > RUNNABLE,在虚拟机内执行的。
> >
> > BLOCKED,受阻塞并等待监视器锁。
> >
> > WATING,无限期等待另一个线程执行特定操作。
> >
> > TIMED_WATING,有时限的等待另一个线程的特定操作。
> >
> > TERMINATED,已退出的。



**使用：**

> `-F`当’jstack [-l] pid’没有相应的时候强制打印栈信息 
>
> `-l`长列表. 打印关于锁的附加信息,例如属于java.util.concurrent的ownable synchronizers列表. 
>
> `-m`打印java和native c/c++框架的所有栈信息. 
>
> `-h` | -help打印帮助信息 
>
> `pid` 需要被打印配置信息的java进程id,可以用jps查询.



ps.有死锁时会有相应提示，或者观察线程的状态来推测：

https://www.hollischuang.com/archives/110







#### 3. javap

javap -c xxx.class

反编译class文件















## Docker

https://docs.docker.com/get-started/overview/

#### Getting start

在项目下创建Dockerfile

> ```dockerfile
> FROM node:12-alpine
> WORKDIR /app
> COPY . .
> RUN yarn install --production
> CMD ["node", "src/index.js"]
> ```
>
> 
>
> ```FROM node:12-alpine``` 
>
> node是镜像名字（docker-hub上查找）,12-alpine是版本号和标签
>
> 
>
> ```WORKDIR /app```
>
> WORKDIR 指定**之后**所有docker命令的工作路径，若不存在，docker会自动创建
>
> 
>
> ```COPY . .```
>
> 调用COPY命令将所有的程序拷贝到Docker镜像中
>
> ```COPY <本地路径> <目标路径>```
>
> 
>
> ```RUN yarn install --production```
>
> RUN允许我们在创建镜像时运行任意的shell命令[如在Linux镜像下，可以运行echo, pwd, rm这些命令]
>
> 
>
> ```CMD ["node", "src/index.js"]```
>
> 最后，用CMD来指定Docker容器运行起来以后执行的命令
>
> 注意：容器 ≠ 镜像
>
> ```CMD ["可执行文件", "参数1", "参数2"...]```
>
> RUN是创建镜像的时候使用的，而CMD是运行容器时候使用的
>
> 
>
> > 创建一个python项目：https://www.bilibili.com/video/BV1s54y1n7Ev?from=search&seid=9764580653505622816&spm_id_from=333.337.0.0
> >
> > ```dockerfile
> > FROM python:3.8-slim-buster
> > WORKDIR /app
> > COPY . .
> > RUN pip3 install -r requirements.txt
> > CMD ["python3", "app.py"]
> > ```
>
> 
>
> 接下来，用 ```docker build``` 来创建一个镜像
>
> ```docker build -t my-finance . ``` 
>
> ```-t : tag / 标签``` :  -t 指定了我们镜像的名字
>
> 最后的  ```.```  告诉docker应该在当前目录下寻找这个dockerfile,不能省略
>
> 
>
> 有了镜像后，通过```docker run``` 来启动一个容器
>
> ```docker run -p 80:5000 -d -my-finance```
>
> -p ： 将容器上的某一个端口映射到本地主机上，80为本地主机端口，后面5000为容器上的端口
>
> -d : (-detached) 让容器在后台运行
>
> 
>
> 通过 **Docker Desktop** 查看应用和输出
>
> -- 删除容器时，新添加的数据也会丢失（像删除虚拟机一样）
>
> 
>
> 常用命令：
>
> > 列举所有的容器： docker ps
> >
> > 停止容器： docker stop <容器id>
> >
> > 重启容器： docker restart <容器id>
> >
> > 删除容器： docker rm <容器id>
> >
> > 启动一个远程shell:  docker exec -it <容器id>   /bin/bash
> >
> > (emmm... 远程那个试下了会报权限错误...)
>
> 
>
> 如果想要保存容器的数据，则需要Docker提供的 **Volumes/数据卷** 
>
> 类似在本地主机和不同容器中的共享文件夹
>
> 创建数据卷：
>
> ```docker volume create my-finace-data```
>
> 启动容器：
>
> ```docker run -dp 80:5000 -v my-finace-data:/etc.finance my-finance```
>
> -v : 指定将这个数据卷挂载（mount）到容器的哪一个路径上
>
> (这里将my-finance-data 挂载到 /etc/finance 这个路径下，向这个路径写入的任何数据都会永久保存在这个数据卷中。)
>
> 
>
> 
>
> Docker compose
>
> 多个容器：
>
> 1. 定义docker-compose.yml
>
>    ```yml
>    version: "3"
>                                                                                                                                  
>    services:
>    	# 通过services来定义多个container
>    	web: 
>    		build:
>    		ports:
>    			- "80:5000"
>    	db:
>    		image: "mysql"
>    		environment: 
>    			MYSQL_DATABASE: finance-db
>    			MYSQL_ROOT_PASSWORD: sss
>    		volumes:
>    			#指定一个数据卷
>    			- my-finance-data:/var/lib/mysql
>    volumes:
>    	my-finance-data:
>    	#.....
>    ```
>    
>     
>    
> 2. 使用 ```docker compose up -d``` 来运行所有容器(-d后台)
>
> 3. 停止：```docker compose down```
>
> 4. 数据卷需要手动删除，或```docker compose down --volumes```
>
>     
>
> 中文教程：https://yeasy.gitbook.io/docker_practice
>
> PS. 扩展：可以了解下 **Kubernetes**









#### Maven构建docker镜像

> 自己网上找资料试的，可能会不对..
>
> https://zhuanlan.zhihu.com/p/89161347



1. 新建一个Springboot项目--(端口我部署在了8001)

2. 加入插件

   ```xml
   <plugin>
       <groupId>com.spotify</groupId>
       <artifactId>dockerfile-maven-plugin</artifactId>
       <version>1.4.13</version>
       <executions>
           <execution>
               <id>default</id>
               <goals>
                   <goal>build</goal>
                   <goal>push</goal>
               </goals>
           </execution>
       </executions>
       <configuration>
           <!--你需要配置的地方-->
   
           <!--指定仓库名/镜像名,这里要改一下-->
           <repository>kennedy/${project.artifactId}</repository>
           <!--指定tag -->
           <tag>${project.version}</tag>
           <buildArgs>
               <!--指定参数jar-->
               <JAR_FILE>${project.build.finalName}.jar</JAR_FILE>
           </buildArgs>
       </configuration>
   </plugin>
   ```

3. 在项目目录下创建Dockerfile

   ```dockerfile
   FROM openjdk:8-jre
   ENTRYPOINT ["java", "-jar", "app.jar"]
   ARG JAR_FILE
   ADD target/${JAR_FILE} app.jar
   ```

4. 用maven的package命令打jar包并docker build

   > mvn package

5. 运行镜像

   > docker run -dp 8001:8001 kennedy/dockerdemo:0.0.1-SNAPSHOT
   >
   > 注意：得映射docker的8001端口才行（对应项目的端口，应该可以配置，但我没试）






























































## 算法

### 前缀树

> Trie（发音类似 "try"）或者说 前缀树 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补完和拼写检查。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/implement-trie-prefix-tree
>
> ps.自己写的



```java
public class Trie {
    private Trie[] children;
    private boolean isEnd;

    /** Initialize your data structure here. */
    public Trie() {
        children = new Trie[26];
        isEnd = false;
    }

    /** Inserts a word into the trie. */
    public void insert(String word) {
        Trie node = this;

        for (int index = 0; index < word.length(); index++) {
            char ch = word.charAt(index);
            int chIndex = ch - 'a';
            
            if (node.children[chIndex] == null) {
                node.children[chIndex] = new Trie();
            }
            node = node.children[chIndex];
        }
        node.isEnd = true;
    }

    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        Trie node = this.nodeSearch(word);
        return node != null && node.isEnd;
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        return this.nodeSearch(prefix) != null;
    }


    private Trie nodeSearch(String word) {
        Trie node = this;

        for (int index = 0; index < word.length(); index++) {
            char ch = word.charAt(index);
            int chIndex = ch - 'a';

            if (node.children[chIndex] == null) {
                return null;
            }
            node = node.children[chIndex];
        }
        return node;
    }

    public Trie[] getChildren() {
        return children;
    }

    public boolean isEnd() {
        return isEnd;
    }
}
```



#### 添加与单词搜索

> 请你设计一个数据结构，支持 添加新单词 和 查找字符串是否与任何先前添加的字符串匹配 。
>
> 实现词典类 WordDictionary ：
>
> WordDictionary() 初始化词典对象
> void addWord(word) 将 word 添加到数据结构中，之后可以对它进行匹配
> bool search(word) 如果数据结构中存在字符串与 word 匹配，则返回 true ；否则，返回  false 。word 中可能包含一些 '.' ，每个 . 都可以表示任何一个字母。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/design-add-and-search-words-data-structure
>
> ps.此问题基于前缀树



```java
class WordDictionary {

    private Trie root;

    public WordDictionary() {
        this.root = new Trie();
    }

    public void addWord(String word) {
        root.insert(word);
    }


    public boolean search(String word) {
        return dfs(word, 0, root);
    }

    private boolean dfs(String word, int index, Trie node) {
        if (index == word.length()) {
            return node.isEnd();
        }

        char ch = word.charAt(index);

        if (Character.isLetter(ch)) {
            int childIndex = ch - 'a';
            Trie child = node.getChild()[childIndex];
            if (child != null && dfs(word, index + 1, child)) {
                return true;
            }
        } else {
            for (int i = 0; i < 26; i++) {
                Trie child = node.getChild()[i];
                if (child != null && dfs(word, index + 1, child)) {
                    return true;
                }
            }
        }
        return false;
    }
}
```





#### 单词搜索II

> https://leetcode-cn.com/problems/word-search-ii/
>
> 给定一个 m x n 二维字符网格 board 和一个单词（字符串）列表 words，找出所有同时在二维网格和字典中出现的单词。
>
> 单词必须按照字母顺序，通过 相邻的单元格 内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。
>
> 
>
> 这个比较难..抄的



```java
class Solution {
    int[][] direction = {{1, 0}, {-1, 0}, {0, -1}, {0, 1}};

    public List<String> findWords(char[][] board, String[] words) {
        Trie trie = new Trie();
        for (String word : words) {
            trie.insert(word);
        }

        Set<String> set = new HashSet<>();
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                dfs(board, trie, i, j, set);
            }
        }

        return new ArrayList<>(set);
    }


    public void dfs(char[][] board, Trie now, int i, int j, Set<String> ans) {
        if (!now.children.containsKey(board[i][j])) {
            return;
        }

        char ch = board[i][j];
        now = now.children.get(ch);
        // 是一个词
        if (!"".equals(now.word)) {
            ans.add(now.word);
        }

        board[i][j] = '#';
        for (int[] dir : direction) {
            int nextI = i + dir[0];
            int nextJ = j + dir[1];
            if (nextI >= 0 && nextI < board.length && nextJ >= 0 && nextJ < board[0].length) {
                dfs(board, now, nextI, nextJ, ans);
            }
        }
        board[i][j] = ch;

    }

    class Trie {
        String word;
        Map<Character, Trie> children;
        boolean isWord;

        public Trie() {
            this.word = "";
            this.children = new HashMap<>();
        }

        public void insert(String word) {
            Trie curr = this;
            for (int i = 0; i < word.length(); i++) {
                char c = word.charAt(i);

                if (!curr.children.containsKey(c)) {
                    curr.children.put(c, new Trie());
                }
                curr = curr.children.get(c);
            }
            curr.word = word;
        }
    }
}
```







### 树

#### 二叉搜索树

二叉搜索树满足如下性质：

- 左子树所有节点的元素值均小于根的元素值；
- 右子树所有节点的元素值均大于根的元素值。











### leetCode题

#### [540. 有序数组中的单一元素](https://leetcode-cn.com/problems/single-element-in-a-sorted-array/)

> 给你一个仅由整数组成的有序数组，其中每个元素都会出现两次，唯有一个数只会出现一次。
>
> 请你找出并返回只出现一次的那个数。
>
> 你设计的解决方案必须满足 O(log n) 时间复杂度和 O(1) 空间复杂度。
>
>  
>
> 示例 1:
>
> 输入: nums = [1,1,2,3,3,4,4,8,8]
> 输出: 2
> 示例 2:
>
> 输入: nums =  [3,3,7,7,10,11,11]
> 输出: 10
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/single-element-in-a-sorted-array

```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int low = 0;
        int high = nums.length - 1;
        while(low < high) {
            int mid = (high - low) / 2 + low;
            mid -= mid & 1;  //mid为偶数
            if(nums[mid] == nums[mid + 1]) {
                low = mid + 2;
            } else {
                high = mid;
            }
        }
        return nums[low];
    }
}
```









































### 其它

> 对数换底公式：
>
> ![img](images\tc_1_01.svg)
>
> 实际上，Java可以：
>
> ```double temp = Math.log(buckets) / Math.log(base);```
>
> 用 ```Math.log()```来求对数
>
>  
>
> ```Math.ceil()``` 对一个数上取整

















## SpringBoot

#### 打JAR包

maven--package ---> 打好的jar包在target下

命令： mvn package

运行： java -jar target/spring-boot-docker-1.0.jar





#### maven命令

* mvn compile 编译,将Java 源程序编译成 ``class``字节码文件。
  
*  mvn test 测试，并生成测试报告
*  mvn clean 将以前编译得到的旧的 class字节码文件删除
* mvn pakage 打包,动态 web工程打 war包，Java工程打 jar 包。
* mvn install 将项目生成 jar 包放在仓库中，以便别的模块调用











## SpringCloud

> 部分来源：https://zhuanlan.zhihu.com/p/43023436

SpringCloud的基础功能：

- 服务治理： Spring Cloud Eureka
- 客户端负载均衡： Spring Cloud Ribbon
- 服务容错保护： Spring Cloud Hystrix
- 声明式服务调用： Spring Cloud Feign
- API网关服务：Spring Cloud Zuul
- 分布式配置中心： Spring Cloud Config

SpringCloud的高级功能:

- 消息总线： Spring Cloud Bus
- 消息驱动的微服务： Spring Cloud Stream
- 分布式服务跟踪： Spring Cloud Sleuth





### 集群/分布式

1. 什么是集群

   集群： **同一个业务，部署在多个服务器上**(不同的服务器运行同样的代码，干同一件事)

   

   > 计算机集群简称集群，是一种计算机系统，它通过一组松散集成的计算机软件和/或硬件连接起来高度紧密地协作完成计算工作。在某种意义上，他们可以被看做作是一台计算机。集群系统中的单个计算机通常称为节点，通常通过局域网连接，但也有其它的可能连接方式。集群计算机通常用来改进单个计算机的计算速度和/或可靠性。一般情况下集群计算机比单个计算机，比如工作站或超级计算机性能价格比要高得多。(来源维基百科)

   集群特点：

   * 通过**多台计算机完成同一个工作**，达到更高的效率。
   * **两机或多机内容、工作过程等完全一样**。如果一台死机，另一台可以起作用。

   好处：

   - 本来只有一台机器处理访问，现在有两台机器处理访问了，**分担了压力**。
   - 如果其中一台忘记缴费了，暂时用不了了。没关系，还有另一台可以用呢。

   

2. 什么是分布式

   分布式： **一个业务分拆多个子业务，部署在不同的服务器上**（不同的服务器，运行不同的代码，为了同一个目的）

   > 分布式系统是一组计算机，通过网络相互连接传递消息与通信后并协调它们的行为而形成的系统。**组件之间彼此进行交互以实现一个共同的目标。**

   

   特点：

   - **功能拆分，模块之间独立**，在使用的时候再将这些**独立的模块组合起来**就是一个系统了。

   好处：

   - 模块之间独立，各做各的事，**便于扩展，复用性高**

   - **高吞吐量**。某个任务需要一个机器运行10个小时，将该任务用10台机器的分布式跑(将这个任务拆分成10个小任务)，可能2个小时就跑完了

     

   （PS.对于访问量打的模块，可以单独提升性能，对于访问量低的服务器可以不提升性能，耦合度更低了）

   

3. 集群/分布式

   集群和分布式并不冲突，可以有**分布式集群**

   

   > 现在3y的公司规模变大了，有5个小伙子写Java，4个小伙子写前端，2个小伙子做测试，1个小伙子做DBA。
   >
   > * Java，前端，测试，DBA的关系看作是分布式的
   >
   > * 5个Java看作是集群的(前端，测试同理)...





### CAP理论

- C：数据一致性(consistency)

- - **所有**节点拥有数据的最新版本

- A：可用性(availability)

- - 数据具备高可用性

- P：分区容错性(partition-tolerance)

- - **容忍网络出现分区**，分区之间网络不可达。



一般我们说的分布式系统，P：分区容错性(partition-tolerance)这个是**必需**的，这是客观存在的。

CAP是无法完全兼顾的，从上面的例子也可以看出，我们可以选AP，也可以选CP。但是，要**注意的是**：不是说选了AP，C就完全抛弃了。不是说选了CP，A就完全抛弃了！

在CAP理论中，**C所表示的一致性是强一致性**(每个节点的数据都是最新版本)，其实一致性还有其他级别的：

- 弱一致性：弱一致性是相对于强一致性而言，它不保证总能得到最新的值；
- 最终一致性(eventual consistency)：放宽对时间的要求，在被调完成操作响应后的某个时间点，被调多个节点的数据最终达成一致









### Eureka

> Eureka 专门用于给其他服务注册的称为Eureka Server（服务注册中心），其它注册到 Eureka Server 的服务称为 Eureka Client。



Eureka Server 一般会这样配置：

```yml
register-with-erueka: false			# false表示不向注册中心注册自己
	fetch-registry: false			# false表示自己都是注册中心，不需要检索服务
```



Eureka Client 分为**服务提供者和服务消费者**。

但很可能，某服务**既是服务提供者又是服务消费者**。



> 如果在网上看到SpringCloud的**某个服务配置没有"注册"到Eureka-Server也不用过于惊讶**(但是它是可以获取Eureka服务清单的)
>
> 很可能只是作者把该服务认作为**单纯的服务消费者**，单纯的服务消费者无需对外提供服务，也就无须注册到Eureka中了
>
> ```yml
> eureka:
> 	client:
> 		register-with-eureka: false
> 		service-url:
> 			defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/
> ```



**Eureka治理机制：**

1. 服务提供者

   > **服务注册**: **启动的时候会通过发送REST请求的方式将自己注册到 Eureka Server 上**，同时带上了自身服务的一些元数据信息。
   >
   > **服务续约**： 在注册完成后，**服务提供者会维护一个心跳**用来持续告诉 Eureka Server： 我还活着
   >
   > **服务下线**： **当服务实例进行正常的关闭操作时，它会触发一个服务下线的REST请求给 Eureka Server**,告诉服务注册中心：我要下线了



2. Eureka Server (服务注册中心):

   > **失效剔除**：默认每隔一段时间（默认为60s），将当前清单中超时（默认为90s）**没有续约的服务剔除出去**。
   >
   > **自我保护**:  Eureka Server在运行期间，会统计心跳失败的比例在15分钟内是否低于85%（通常由于网络不稳定导致）。Eureka Server 会将当前的**实例注册信息保护起来**，让这些实例不会过期，尽可能**保护这些注册信息**。







<img src="\images\tc_1_02.jpg" alt="preview" style="zoom: 67%;" />







### RestTemplate 和 Ribbon

**RestTemplate**

通过Eureka服务治理框架，我们可以通过服务名来获取具体的服务实例的位置了(IP)。一般在使用SpringCloud的时候**不需要自己手动创建**HttpClient来进行远程调用。

可以使用Spring封装好的**RestTemplate**工具类，使用起来很简单：

```java
// 传统的方式，直接显示写死IP是不好的！
//private static final String REST_URL_PREFIX = "http://localhost:8001";

// 服务实例名
private static final String REST_URL_PREFIX = "http://MICROSERVICECLOUD-DEPT";

/**
 * 使用 使用restTemplate访问restful接口非常的简单粗暴无脑。 (url, requestMap,
 * ResponseBean.class)这三个参数分别代表 REST请求地址、请求参数、HTTP响应转换被转换成的对象类型。
 */
@Autowired
private RestTemplate restTemplate;

@RequestMapping(value = "/consumer/dept/add")
public boolean add(Dept dept) {
    return restTemplate.postForObject(REST_URL_PREFIX + "/dept/add", dept, Boolean.class);
}
```







**Ribbon**

> 为了实现微服务的高可用，我们可以将服务提供者集群。比如说，现在一个秒杀系统设计出来了，准备上线了。在11月11号时为了能够支持高并发，我们开多台机器来支持并发量。
>
> 现在想要这三个秒杀系统**合理摊分**用户的请求(专业来说就是负载均衡)，可能你会想到nginx。
>
> 其实SpringCloud也支持的负载均衡功能，只不过它是**客户端的负载均衡**，这个功能实现就是Ribbon！



负载均衡又区分了两种类型：

* 客户端负载均衡（Ribbon）
  * 服务实例的**清单在客户端**，客户端进行负载均衡算法分配
  * （从上面的知识我们已经知道了：客户端可以从Eureka Server 中得到一份服务清单，在发送请求时想通过负载均衡算法，**在多个服务器之间选择一个进行访问**）
* 服务端负载均衡（Nginx）
  * 服务实例的**清单在服务端**，服务端进行负载均衡算法分配



<img src="images\tc_1_03.jpg" alt="preview" style="zoom:80%;" />







Ribbon是支持负载均衡，**默认的负载均衡策略是轮询**，我们也是可以根据自己实际的需求自定义负载均衡策略的。

```java
@Configuration
public class MySelfRule {
    @Bean
    public IRule myRule() {
        //return new RandomRule();// Ribbon默认是轮询，我自定义为随机
		//return new RoundRobinRule();// Ribbon默认是轮询，我自定义为随机
        return new RandomRule_ZY();// 我自定义为每台机器5次
    }
}
```



实现起来也简单：继承```AbstractLoadBalancerRule``` 类，重写 ```public Server Choose(ILoadBalancer lb, Object key)``` 即可。



SpringCloud 在CAP理论是选择了AP的，在Ribbon中还可以配置**重试机制**的







### Hysrtrix





































## 设计模式
### 单例模式
#### 单例模式如何保证线程安全

> 参考博客：
>
> https://www.cnblogs.com/kubidemanong/p/9665339.html



##### 懒汉模式实现单例模式：

> *懒汉模式：就是等到有线程调用getInstance这个方法时，才来创建对象实例。与懒汉模式相反的是***饿汉模式**

```java
public class Singleton {
    private Singleton instance = null;
    //私有构造函数
    private Singleton(){};
    
    public Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```





##### 双重检测机制：

基本能保证线程安全

> *解释说明：当instance==null时，假如有两个线程p1,p2进入了第一个if语句，之后p1进入的同步块中，成功创建了对象实例，这时候论到p2进入同步块，由于同步块还有一层if(instance==null)的判断，又因为此时instance != null了，所以p2无法再创建新的实例对象。*



```java
public class Singleton {
    private Singleton instance = null;
    //私有构造函数
    private Singleton(){};
    
    //双重检测机制
    public Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```





> 其实这个线程安全的问题，主要是因为对象的创建过程并非是**原子性**的。在创建的过程中，由于**指令重排**的影响，才导致出现问题的。
>
> > 所谓指令重排就是改变了指令的执行顺序，例如代码中有两行代码：int a = 10;int b = 20;由于虚拟机指令重排的影响，编译后有可能顺序被改变了，变成这样：int b = 20;int a = 10;
>
> 
>
> 且听我慢慢道来：
>
> 当我们的虚拟机在执行 instance = new Singleton这句代码时，会被分解成以下三个动作来执行：
>
> memory = allocate();//1: 给对象分配内存空间。
>
> ctorInstance(memory);//2: 初始化对象
>
> instance = memory; //3: 把instance变量指向刚刚分配的内存地址。
>
> 
>
> 但是，这三个动作的执行顺序并非是一成不变的，有可能经过JVM和CPU的优化编译之后，这三个动作的执行顺序发生了改变，变成了这样：
>
> memory = allocate();//1: 给对象分配内存空间。
>
> instance = memory; //3: 把instance变量指向刚刚分配的内存地址
>
> ctorInstance(memory);//2: 初始化对象
>
> 
>
> 现在假设instance== null,且有p1, p2两个线程来调用这个方法。当p1执行完1，3但还没有执行2时，这时instance已经不再是null了。假如这个时候p2刚刚进入getInstance这个方法，然后执行if(instance == null)的判断语句，这个时候判断的结果会是false，于是p2直接把instance给返回的。
>
> 但由于p1还没有执行动作2，此时的对象还没有被初始化，但却已经被p2给返回了。此时，这个被返回的对象出现问题了。
>
> 于是，就出现了线程安全问题。





##### 通过volatile来保证指令重排问题

> 问题的根源就是指令重排的影响，所以我们只要保证在创建对象的时候，不要出现指令重排就可以了。
>
> 所以说，我们可以把instance这个变量声明为volatile



```java
public class Singleton {
    private static volatile Singleton instance = null;
    //私有构造函数
    private Singleton(){};
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```



> 关于 ```volatile``` 关键字：
>
> https://mp.weixin.qq.com/s?__biz=Mzg2NzA4MTkxNQ==&mid=2247485258&idx=1&sn=377e6be99cf63e028c98fab56dbaa490&source=41#wechat_redirect






##### 懒汉式的缺点

> **懒汉式**这种方式需要我们来自己加锁，保证线程安全的问题。
>
> 不过就算我们保证了线程安全，这种写法还是无法保证存在唯一一个对象实例。因为别人还是可以通过**反射**的方式来创建一个新的对象。



```java
public class Singleton {
    public static void main(String[] args) throws Exception {
        //获取构造器
        Constructor<Singleton> c = Singleton.class.getDeclaredConstructor();
        //把构造器设置为可访问
        c.setAcessible(true);
        //创建两个示例对象
        Singleton s1 = c.newInstance();
        Singleton s2 = c.newInstance();
        //比较下两个实例是否相等
        System.out.println(s1 == s2);
    }
}

//>>>result:  false
```





##### 饿汉式

> 所谓饿汉式，就是一开始把对象实例创建出来，而不是等getInstace这个方法被调用才来创建对象。

```java
public class Singleton2 {
    private static Singleton2 instance = new Singleton2();
    
    //私有构造器
    private Singleton2(){};
    
    public static Sinleton2 getInstance() {
        return instance;
    }
}
```



> 总结一下饿汉式的一些问题：
>
> 1、有可能出现对象白白浪费的情况。
>
> 2、和懒汉式一样，无法处理反射问题。





##### 采用静态内部类的写法

```java
public class Singleton3 {
    //静态内部类
    private static class LazyHolder {
        private static Singleton3 instance = new Singleton3();
    }
    
    //私有构造器
    private Singleton3(){};
    
    public static Singleton3 getInstace() {
        return LayHolder.instance;
    }
}
```



> 由于外部类无法访问静态内部类，因此只有当外部类调用Singleton.getInstance()方法的时候，才能得到instance实例。
>
> 并且，instance实例对象初始化的时机并不是在Singleton被加载的时候，而是当getInstance()方法被调用的时候，静态内部类才会被加载，这时instance对象才会被初始化。并且也是线程安全的。
>
> 所以，与饿汉式相比，通过**静态内部类**的方式，可以保证instance实例对象不会被白白浪费。
>
> 但是，它仍然存在**反射**问题。







##### 枚举方式

```java
public enum Singleton4 {
    INSTANCE;
}
```



> 枚举的方式简单吧？一行代码就搞定了，不过和饿汉式一样，由于一开始instance实例就被创建了，所以有可能出现白白浪费的情况。
>
> 但是，通过枚举的方式，不仅代码简单，线程安全，而且JVM还能阻止**反射**获取枚举类的私有构造器。
>
> 下面做个实验



```java
public enum Singleton4 {
    //一般用大写的了，不过为了和前面的统一
    //我就用小写的了
    instance;

    public static void main(String[] args) throws Exception{
        //获得构造器
        Constructor<Singleton4> c = Singleton4.class.getDeclaredConstructor();
        //把构造器设置为可访问
        c.setAccessible(true);
        //创建两个实例对象
        Singleton4 s1 = c.newInstance();
        Singleton4 s2 = c.newInstance();
        //比较下两个实例是否相等
        System.out.println(s1 == s2);
    }
}
```



结果出现了异常：

Exception in thread “main” java.lang.NoSuchMethodException: singleton.Singleton4.()

```text
at java.lang.Class.getConstructor0(Class.java:3082)

at java.lang.Class.getDeclaredConstructor(Class.java:2178)

at singleton.Singleton4.main(Singleton4.java:12)
```







### 观察者模式

> 观察者（Observer）模式的定义：指多个对象间存在一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。这种模式有时又称作发布-订阅模式、模型-视图模式，它是对象行为型模式。
>
> 观察者模式是一种对象行为型模式，其主要优点如下。
>
> 1. 降低了目标与观察者之间的耦合关系，两者之间是抽象耦合关系。符合依赖倒置原则。
> 2. 目标与观察者之间建立了一套触发机制。
>
> 
> 它的主要缺点如下。
>
> 1. 目标与观察者之间的依赖关系并没有完全解除，而且有可能出现循环引用。
> 2. 当观察者对象很多时，通知的发布会花费很多时间，影响程序的效率。
>
>  
>
> 观察者模式的主要角色如下。
>
> 1. 抽象主题（Subject）角色：也叫抽象目标类，它提供了一个用于保存观察者对象的聚集类和增加、删除观察者对象的方法，以及通知所有观察者的抽象方法。
> 2. 具体主题（Concrete Subject）角色：也叫具体目标类，它实现抽象目标中的通知方法，当具体主题的内部状态发生改变时，通知所有注册过的观察者对象。
> 3. 抽象观察者（Observer）角色：它是一个抽象类或接口，它包含了一个更新自己的抽象方法，当接到具体主题的更改通知时被调用。
> 4. 具体观察者（Concrete Observer）角色：实现抽象观察者中定义的抽象方法，以便在得到目标的更改通知时更新自身的状态。



-- Android项目中观察者模式：

```java
public interface Observer {
    void onEvent(int eventType);
    boolean intercept();
}
```



> 仅参考

```java
public class Bus {
    // SpareArray中key为int类型，value为Object类型
    private final static SpareArray<LinkedList<Reference<Observer>>> eventMap = new SpareArray<>();
    
    //订阅
    public static void register(final int e, final Observer o) {
        if (null == o || o.intercept()) return;

        synchronized (eventMap) {
            LinkedList<Reference<Observer>> l = eventMap.get(e);

            if (null == l) {
                l = new LinkedList<>();
                eventMap.put(e, l);
                l.add(new WeakReference<>(o));
            } else {
                Reference<Observer> ref = null;

                Iterator<Reference<Observer>> it = l.iterator();

                while (it.hasNext()) {
                    ref = it.next();
                    if (null == ref || null == ref.get() || ref.get().intercept()) {
                        it.remove();
                    } else if (ref.get() == o) {
                        return;
                    }
                }

                l.add(new WeakReference<>(o));
            }
        }
    }


    /**
     * 取消订阅事件
     * 不在强制要求
     * 注册之后，不取消也问题不大
     */
    public static void unRegister(final int t, final Observer o) {
        synchronized (eventMap) {

            LinkedList<Reference<Observer>> list = eventMap.get(t);

            if (null == list) return;
            if (0 == list.size()) return;

            Iterator<Reference<Observer>> it = list.iterator();
            Reference<Observer> ref = null;

            while (it.hasNext()) {
                ref = it.next();
                if (null == ref || null == ref.get() || ref.get().intercept()) {
                    it.remove();
                } else if (ref.get() == o) {
                    it.remove();
                    break;
                }
            }
        }
    }


    //在UI线程 发布
    public static void notify(final int e) {
        //GlobalHanler中也是调用了Android中的Handler
        GlobalHandler.post(new Runnable() {
            @Override
            public void run() {
                synchronized (eventMap) {
                    LinkedList<Reference<Observer>> l = eventMap.get(e);
                    if (null == l) return;
                    if (0 == l.size()) return;

                    Iterator<Reference<Observer>> it = l.iterator();

                    Reference<Observer> ref = null;

                    while (it.hasNext()) {
                        ref = it.next();
                        if (null == ref || null == ref.get() || ref.get().intercept()) {
                            it.remove();
                        } else {
                            try {
                                ref.get().onEvent(e);
                            } catch (Throwable t) {
                                it.remove();
                            }
                        }
                    }
                }
            }
        });
    }

    /**
     * 删除所有的事件订阅
     * */
    public static void clear() {
        synchronized (eventMap) {
            eventMap.clear();
        }
    }
}
```









## SQL

### other

-- 在group by 中 对 日期进行 order by 倒序排序

​	(用MAX() 即可)

```sql
SELECT
	GROUP_CONCAT( id ) AS id,
	itemCode,
	itemName,
	barcode,
-- 	MAX(lastUpdated),
	SUM( convertedQty ) AS qty 
FROM
	shipping_container_detail 
WHERE
	warehouseCode = 'JD-01'
-- 	AND storeCode =? 
-- 	AND slipwayCode = ? 
-- 	AND trayCode = ? 
	AND STATUS = 500 
	AND (userDef1 IS NULL OR userDef1 <> 'SCATTERED_PICK')
	AND builtLpnStatus = 0  
	AND ( `temporary` = 1 OR shipmentType = 'YKRK' ) 
GROUP BY
	itemCode
ORDER BY 
	MAX(lastUpdated) DESC;
	
```







**Navicat导出数据库**

> 直接在 “工具” -> "数据传输"，这方法0错误
>
> ps.，要是导出为db文件再导回去会有一些报错























## 高并发

> 参考《Java高并发程序设计 第二版》



### 概念

**并发（Concurrency) 和 并行（Pareallelism）**

> 并发和并行都可以表示两个或者多个任务一起执行，但是侧重点有所不用。
>
> 并发偏重于多个任务**交替**执行，而多个任务质检可能是串行的。
>
> 并行是真正意义上的 “同时执行”。





**临界区**

> 临界区用来表示一种公共资源或者说共享数据，可以被多个线程使用。但是每一次，只能有一个线程使用它。一旦临界区资源被占用，其他线程要想使用这个资源就必须等待。
>
> 在并行程序中，临界区资源是保护的对象。





**阻塞（Blocking）和 非阻塞 （Non-Blocking）**

> 阻塞和非阻塞通常用来形容多线程间的相互影响。
>
> 比如一个线程占用了临界区资源，那么其他所有需要这个资源的线程就必须在这个临界区中等待。等待会导致线程挂起，这种情况就是阻塞。
>
> 非阻塞的意思与之相反，它强调没有一个线程可以妨碍其他线程执行，所有的线程都会尝试不断前向执行。









**死锁（Deadlock), 饥饿（Starvation）和活锁（Livelock)**

> 死锁是指两个或两个以上的进程在执行过程中，由于竞争资源或者由于彼此通信而造成的一种阻塞的现象。
>
> 饥饿是指某一个或者多个线程因为种种原因无法获得所需要的资源，导致一直无法执行。（比如它的线程优先级太低）
>
> 活锁指的是任务或者执行者没有被阻塞，由于某些条件没有满足，导致一直重复尝试—失败—尝试—失败的过程。（指两个线程，都主动将资源释放出去给别的线程使用，导致资源不断在线程间跳动，最终没有一个线程可以同时拿到所有资源正常执行）







**并发级别**

> 阻塞、无饥饿、无阻碍、无锁、无等待







**Java内存模型（JMM）**

> JMM的关键技术都是围绕多线程的原子性，可见性，有序性来建立的。
>
> **原子性**
>
> 原子性是指一个操作是不可中断的。即使是在多个线程一起执行的时候，一个操作一旦开始，就不会被其他线程干扰。
>
> **可见性**
>
> 可见性是指当一个线程修改了某一个共享变量的值是，其它线程是否能够立即知道这个修改。
>
> **有序性**
>
> （指令重排）









## RabbitMQ

### 常用命令

> 1、以应用方式启动 
>
> rabbitmq-server -detached 后台启动
>
>  Rabbitmq-server 直接启动，如果你关闭窗口或者需要在改窗口使用其他命令时应用就会停止  
>
> 关闭:rabbitmqctl stop
>
>  2、以服务方式启动（安装完之后在任务管理器中服务一栏能看到RabbtiMq） 
>
> rabbitmq-service install 安装服务 
>
> rabbitmq-service start 开始服务 
>
> Rabbitmq-service stop 停止服务 
>
> Rabbitmq-service enable 使服务有效
>
>  Rabbitmq-service disable 使服务无效 







### Virtual host

> 参考文档： https://www.cnblogs.com/hopher/p/7644718.html
>
> 可以应用在不同的项目上



​	什么是virtual_host

> vhost是rabbitmq分配权限的最小细粒度。比如我们可以为一个用户分配一个可以访问哪个或者哪一些vhost的权限。
> 但是不能为用户分配一个可以访问哪一些exchange，或者queue的权限，因为rabbitmq的权限细粒度没有细化到交换器和队列，他的最小细粒度是vhost(vhost中包含许多的exchanges，queues，bingdings)。
> 所以如果exchangeA 和queueA 只能让用户A访问，exchangeB 和queueB 只能让用户B访问，要达到这种需求，只能为exchangeA 和queueA创建一个vhostA，为exchangeB 和queueB 创建vhostB，这样就隔离开来了。
>
> 补充：一个broker可以开设多个vhost，用于不同用户的权限分离
>
>  virtual host只是起到一个命名空间的作用，所以可以多个user共同使用一个virtual host，文章开头写的vritual_host = '/'，这个是系统默认的



​    相关命令：

> ps.可以直接在localhost:15672上操作
>
> * 查看所有虚拟主机
>
>   ```bash
>   rabbitmqctl list_vhosts
>   ```
>
> * 添加vhost
>
>   ```bash
>   rabbitmqctl add_vhost test_vhost
>   rabbitmqctl list_vhosts
>   ```
>
> * add_user添加用户
>
>   ```bash
>   rabbitmqctl add_user test1 123456
>   rabbitmqctl add_user test2 123456
>   rabbitmqctl list_users
>   ```
>
>   ps.用户管理命令
>
>   ```bash
>   add_user <username> <password>
>   delete_user <username>
>   change_password <username> <newpassword>
>   clear_password <username>
>   authenticate_user <username> <password>
>   set_user_tags <username> <tag> ...
>   list_users
>   ```
>
> * set_permissions 分配访问
>
>   ```bash
>   set_perssions [-p <vhost>] <user> <conf> <write> <read>
>   ```
>
>   其中，权限控制（配置，读，写） 的位置分别用正则表达式来匹配特定的资源，如'^(amq.gen.*|amq.default)'可以匹配server生成的和默认的exchange，'^'可以匹配server生成的和默认的exchange，'^'不匹配任何资源
>
>   > 需要注意的是RabbitMQ会缓存每个connection或channel的权限验证结果、因此权限发生变化后需要重连才能生效。
>
>   ```bash
>   Copyrabbitmqctl set_permissions -p test_host  test1 ".*" ".*" ".*"
>   rabbitmqctl set_permissions -p test_host  test2 ".*" ".*" ".*"
>   ```





















## XMind

### 操作

```tab``` 添加子主题

```Enter``` 添加同级主题

画流程图---用自定义主题，然后建立关系，把关系的线的样式改一改

建立画布链接：对一主题，右键--从主题新建画布



















## Powershell

> Powershell上禁止运行脚本：
>
> https://docs.microsoft.com/zh-cn/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.2
>
> ```cmd
> Get-ExecutionPolicy
> //获取到当前执行策略为Restricated
> ```
>
> ### Restricted
>
> - Windows 客户端计算机的默认执行策略。
> - 允许单个命令，但不允许脚本。
> - 阻止运行所有脚本文件，包括格式化和配置文件 () 、模块脚本文件 () 和 `.ps1xml` `.psm1` PowerShell 配置文件 `.ps1` () 。
>
> ### RemoteSigned
>
> - Windows 服务器计算机的默认执行策略。
> - 脚本可以运行。
> - 需要受信任的发布者对从 Internet 下载的脚本和配置文件（包括电子邮件和即时消息程序）进行数字签名。
> - 对于在本地计算机上编写且未从 Internet 下载的脚本，不需要数字签名。
> - 如果脚本未受阻止（例如使用 cmdlet），则运行从 Internet 下载且 `Unblock-File` 未签名的脚本。
> - 从 Internet 来源（而不是 Internet）运行未签名脚本的风险，以及可能是恶意的已签名脚本的风险。
>
> ```cmd
> //在管理员模式下执行
> Set-ExecutionPolicy RemoteSigned
> ```















## 环境

### Jdk

**多版本jdk**

> 安装JDK
>
> > ps.(自己记录)战66中，jdk安装位置：
> >
> > 1. C:\Java\oraclejdk_17\jdk-17
> > 2. C:\Program Files\Java\jdk1.8.0_271

> 删除 C:\Program Files (x86)\Common Files\Oracle\Java\javapath 下的 java.exe, javaw.exe, javaws.exe
>
> 删除环境变量path下的C:\Program Files (x86)\Common Files\Oracle\Java\javapath
>
> 配置环境变量```JAVA_HOME```
>
> 配置```classpath = .;%JAVA_HOME%\lib;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar```
>
> 配置```path``` 添加： ```%JAVA_HOME%\bin``` , ```%JAVA_HOME%\jre\bin```

重新设置JAVA_HOME路径即可



可以搞个脚本```javahome.bat```，切换JAVA_HOME

```bash
IF %1 EQU 8 (
	setx "JAVA_HOME" "C:\Program Files\Java\jdk1.8.0_271" /m
	echo "JAVA_HOME" has been modified C:\Program Files\Java\jdk1.8.0_271
) ELSE IF %1 EQU 17 (
	setx "JAVA_HOME" "C:\Java\oraclejdk_17\jdk-17" /m
	echo "JAVA_HOME" has been modified C:\Java\oraclejdk_17\jdk-17
) ELSE (
	echo Not a exist Java version.
)
```



![image-20211209174210617](images\tc_1_04.png)









### Springboot

> 启动时报错：Error running 'Application': Command line is too long
>
> 在IDEA/workspace.xml文件，```<component name="PropertiesComponent"></component >``` 标签下，加上：
>
> ```xml
> <property name=“dynamic.classpath" value="true" />
> ```
>
> 

















### Git配置

> 项目出现 connect refused的问题

解决方法：

> 找到项目的路径配置，找到git文件夹，找到config，编辑
>
> 找到url参数：把http改为https

























## 脚本

### Shell

> Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。
>
> Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。
>
> Ken Thompson 的 sh 是第一种 Unix Shell，Windows Explorer 是一个典型的图形界面 Shell。



> Shell 编程跟 JavaScript、php 编程一样，只要有一个能编写代码的文本编辑器和一个能解释执行的脚本解释器就可以了。
>
> Linux 的 Shell 种类众多，常见的有：
>
> - Bourne Shell（/usr/bin/sh或/bin/sh）
> - Bourne Again Shell（/bin/bash）
> - C Shell（/usr/bin/csh）
> - K Shell（/usr/bin/ksh）
> - Shell for Root（/sbin/sh）
>
> 默认为Bash
>
> 在一般情况下，人们并不区分 Bourne Shell 和 Bourne Again Shell，所以，像 **#!/bin/sh**，它同样也可以改为 **#!/bin/bash**。
>
> **#!** 告诉系统其后路径所指定的程序即是解释此脚本文件的 Shell 程序。



**hello world**

```php
#!/bin/bash
echo "Hello world !"
```



**运行Shell脚本**

方法1： 作为可执行程序

> 将代码保存为test.sh, cd到相应的目录
>
> ```bash
> chmod +x ./test.sh    #使脚本具有执行权限
> ./test.sh    #执行脚本
> ```
>
> 注意，一定要写成 **./test.sh**，而不是 **test.sh**，运行其它二进制的程序也一样，直接写 test.sh，linux 系统会去 PATH 里寻找有没有叫 test.sh 的，而只有 /bin, /sbin, /usr/bin，/usr/sbin 等在 PATH 里，你的当前目录通常不在 PATH 里，所以写成 test.sh 是会找不到命令的，要用 ./test.sh 告诉系统说，就在当前目录找。



方法2： 作为解析器参数

> 这种运行方法是，直接运行编译器，其参数就是shell脚本的文件名，如：

```bash
/bin/sh test.sh
/bin/php test.php
```

> 这种方式运行的脚本，不需要在第一行指定解释器信息，写了也没用。





#### Shell变量

>定义变量时，变量名不加美元符号（$，PHP语言中变量需要），如：
>
>```
>your_name="runoob.com"
>```
>
>注意，变量名和等号之间不能有空格，这可能和你熟悉的所有编程语言都不一样。同时，变量名的命名须遵循如下规则：
>
>- 命名只能使用英文字母，数字和下划线，首个字符不能以数字开头。
>- 中间不能有空格，可以使用下划线 **_**。
>- 不能使用标点符号。
>- 不能使用bash里的关键字（可用help命令查看保留关键字）。
>
>有效的 Shell 变量名示例如下：
>
>```
>RUNOOB
>LD_LIBRARY_PATH
>_var
>var2
>```
>
>无效的变量命名：
>
>```
>?var=123
>user*name=runoob
>```
>
>除了显式地直接赋值，还可以用语句给变量赋值，如：
>
>```
>for file in `ls /etc`
>或
>for file in $(ls /etc)
>```
>
>以上语句将 /etc 下目录的文件名循环出来。



**使用变量**

使用一个定义过的变量，只要在变量名前面加美元符号即可，如：

```bash
your_name="qinjx"
echo $your_name
echo ${your_name}
```

变量名外面的花括号是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界，比如下面这种情况：

```bash
for skill in Ada Coffee Action Java; do
	echo "I am good at ${skill}script"
done
```

如果不给skill变量加花括号，写成echo "I am good at $skillScript"，解释器就会把$skillScript当成一个变量（其值为空），代码执行结果就不是我们期望的样子了。

推荐给所有变量加上花括号，这是个好的编程习惯。

已定义的变量，可以被重新定义，如：

```
your_name="tom"
echo $your_name
your_name="alibaba"
echo $your_name
```

这样写是合法的，但注意，第二次赋值的时候不能写$your_name="alibaba"，使用变量的时候才加美元符（$）。





**只读变量**

使用 readonly 命令可以将变量定义为只读变量，只读变量的值不能被改变。

下面的例子尝试更改只读变量，结果报错：

```
#!/bin/bash
myUrl="https://www.google.com"
readonly myUrl
myUrl="https://www.runoob.com"
```

运行脚本，结果如下：

```
/bin/sh: NAME: This variable is read only.
```





**删除变量**

使用 unset 命令可以删除变量。语法：

```
unset variable_name
```

变量被删除后不能再次使用。unset 命令不能删除只读变量。

**实例**

```
#!/bin/sh
myUrl="https://www.runoob.com"
unset myUrl
echo $myUrl
```

以上实例执行将没有任何输出。



**变量类型**

运行shell时，会同时存在三种变量：

- **1) 局部变量** 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
- **2) 环境变量** 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
- **3) shell变量** shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行









**Shell字符串**

> 字符串是shell编程中最常用最有用的数据类型（除了数字和字符串，也没啥其它类型好用了），字符串可以用单引号，也可以用双引号，也可以不用引号。

**单引号**

```
str='this is a string'
```

单引号字符串的限制：

- 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
- 单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。





**双引号**

```bash
your_name="runoob"
str="Hello, I know you are \"$your_name\"! \n"
echo -e $str
```

> echo:
>
> -e：激活转义字符



输出结果为：

```
Hello, I know you are "runoob"! 
```

双引号的优点：

- 双引号里可以有变量
- 双引号里可以出现转义字符







**拼接字符串**

```bash
your_name="runoob"
# 使用双引号拼接
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
echo $greeting  $greeting_1
# 使用单引号拼接
greeting_2='hello, '$your_name' !'
greeting_3='hello, ${your_name} !'
echo $greeting_2  $greeting_3
```

输出结果为：

```bash
hello, runoob ! hello, runoob !
hello, runoob ! hello, ${your_name} !
```



**获取字符串长度**

```
string="abcd"
echo ${#string} #输出 4
```



**提取子字符串**

以下实例从字符串第 **2** 个字符开始截取 **4** 个字符：

```
string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
```

**注意**：第一个字符的索引值为 **0**。



**查找子字符串**

查找字符 **i** 或 **o** 的位置(哪个字母先出现就计算哪个)：

```
string="runoob is a great site"
echo `expr index "$string" io`  # 输出 4
```

**注意：** 以上脚本中 **`** 是反引号，而不是单引号 **'**，不要看错了哦。







**Shell 数组**

bash支持一维数组（不支持多维数组），并且没有限定数组的大小。

类似于 C 语言，数组元素的下标由 0 开始编号。获取数组中的元素要利用下标，下标可以是整数或算术表达式，其值应大于或等于 0。



**定义数组**

在 Shell 中，用括号来表示数组，数组元素用"空格"符号分割开。定义数组的一般形式为：

```
数组名=(值1 值2 ... 值n)
```

例如：

```bash
array_name=(value0 value1 value2 value3)
```

或者

```bash
array_name=(
value0
value1
value2
value3
)
```

还可以单独定义数组的各个分量：

```bash
array_name[0]=value0
array_name[1]=value1
array_name[n]=valuen
```

可以不使用连续的下标，而且下标的范围没有限制。



**读取数组**

读取数组元素值的一般格式是：

```
${数组名[下标]}
```

例如：

```
valuen=${array_name[n]}
```

使用 **@** 符号可以获取数组中的所有元素，例如：

```
echo ${array_name[@]}
```



**获取数组的长度**

获取数组长度的方法与获取字符串长度的方法相同，例如：

```bash
# 取得数组元素的个数
length=${#array_name[@]}
# 或者
length=${#array_name[*]}
# 取得数组单个元素的长度
lengthn=${#array_name[n]}
```









**Shell 注释**

以 **#** 开头的行就是注释，会被解释器忽略。

通过每一行加一个 **#** 号设置多行注释，像这样：

```
#--------------------------------------------
# 这是一个注释
# author：菜鸟教程
# site：www.runoob.com
# slogan：学的不仅是技术，更是梦想！
#--------------------------------------------
##### 用户配置区 开始 #####
#
#
# 这里可以添加脚本描述信息
# 
#
##### 用户配置区 结束  #####
```

如果在开发过程中，遇到大段的代码需要临时注释起来，过一会儿又取消注释，怎么办呢？

每一行加个#符号太费力了，可以把这一段要注释的代码用一对花括号括起来，定义成一个函数，没有地方调用这个函数，这块代码就不会执行，达到了和注释一样的效果。



**多行注释**

多行注释还可以使用以下格式：

```bash
:<<EOF
注释内容...
注释内容...
注释内容...
EOF
```

EOF 也可以使用其他符号:

```bash
:<<'
注释内容...
注释内容...
注释内容...
'

:<<!
注释内容...
注释内容...
注释内容...
!
```





### 发版逻辑

> 此处只记录TTX的发版逻辑

1. 在Jenkins上（https://ci.ittx.com.cn/） 管理项目，进行build操作

2. 在服务器上，找到tomcat-deploy，运行脚本，从服务器上下载代码下来，部署到tomcat

   ![image-20220110182844694](images\tc_1_05.png)





































## Git

> 略
>
>  ###2022-05-27 : 见tc3



> 合并分支错了如何退回？
>
> 1. ```git reflog``` 查看历史版本
>
> 2. ```git reset --hard head@{n}```      来选择回退的版本
>
>    或者 ```git reset --hard 版本id```    也可以









































































































## Unity

> https://www.bilibili.com/video/BV1PL4y1e7hy?spm_id_from=444.41.0.0
>
> 有空去玩玩~









