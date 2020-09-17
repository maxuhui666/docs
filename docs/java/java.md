## Java 8 使用 Lambda 表达式操作集合

### 遍历集合

```java
// Java 8 新特性，创建集合时初始化。 Java 9 中可以直接使用 List.of("小明", "小花", "小李")
List<String> list = Stream.of("小明", "小花", "小李").collect(toList());

// Java 8 之前的写法
for (String name : list) {
    System.out.println(name);
}

// Java 8 的写法
list.forEach(name -> System.out.println(name));

// 更简洁的写法，方法引用由 :: 双冒号操作符表示 （若对参数有任何修改，则不能使用方法引用）
list.forEach(System.out::println);

// 遍历 Map 
Map<Integer, String> map = new HashMap<Integer, String>(3) {
    {
        put(1, "小明");
        put(2, "小花");
        put(3, "小李");
    }
};

// Java 8 之前的写法
for (Integer key : map.keySet()) {
	System.out.println(key + " = > " + map.get(key));
}

// Java 8 的写法
map.forEach((k, v) -> System.out.println(k + " = > " + v));

```

### 一行代码求集合最大值、最小值、总和和平均值

```java
// 获取数字的个数、最小值、最大值、总和以及平均值
List<Integer> primes = Stream.of(2, 3, 5, 7, 11, 13, 17, 19, 23, 29).collect(toList());
// 一行代码求集合最大值、最小值、总和、平均值
IntSummaryStatistics stats = primes.stream().mapToInt((x) -> x).summaryStatistics();
System.out.println("最大值 " + stats.getMax());
System.out.println("最小值 " + stats.getMin());
System.out.println("总 和 " + stats.getSum());
System.out.println("平均值: " + stats.getAverage());
```

### 集合排序

```java
// 普通 int 值
List<Integer> array = Stream.of(3, 2, 4, 6).collect(toList());

// 升序排序
array.sort(Integer::compareTo);
```

```java
// 一个用户实体类
@Data
public class User implements Serializable {
    private String name;
    private Integer age;
    private Integer salary;
}
```

```java
// 实体类集合
List<User> userList = Stream.of(
        new User("小明", 25, 100),
        new User("小花", 22, 80),
        new User("小李", 22, 79))
        .collect(Collectors.toList());

// 按照指定属性排序 (如果想倒序，再后面调用.reversed()即可)
userList.sort(Comparator.comparing(User::getAge));
// 先按照年龄排序，如果年龄相同，再按照薪资排序 
userList.sort(Comparator.comparing(User::getAge).thenComparing(User::getSalary));
```

### 集合分组

```java
// 按照年龄进行分组
Map<Integer, List<User>> map = userList.stream().collect(Collectors.groupingBy(User::getAge));

// 输出结果
22 = > [User{name='小花', age=22, salary=80}, User{name='小李', age=22, salary=79}]
25 = > [User{name='小明', age=25, salary=100}]
```

### 集合过滤

```java
// 过滤掉年龄是22岁的
userList.stream().filter(u -> u.getAge() != 22).collect(toList());
// 过滤掉姓王的
userList.stream().filter(u -> !u.getName().startsWith("王")).collect(toList());
// 过滤掉年薪100w以下的人
userList.stream().filter(u -> u.getSalary() >= 100).collect(toList());
// 过滤掉年薪200w以上的和姓王的人
userList.stream().filter(u -> u.getSalary() <= 200 && !u.getName().startsWith("王")).collect(toList());

// 此处省略更多例子...
```

### 其他操作

```java
// 在集合中查找是否有名字叫小明的 User 对象
userList.stream().anyMatch(u -> "小明".equals(u.getName()));
// 集合中没有小兰
userList.stream().noneMatch(u -> "小兰".equals(u.getName()));
```
