**1. 为什么要克隆，为何不直接new一个对象？**

new出来的是一个全新的对象，而克隆是为了可以保存一些已经修改过的属性或状态，方便进行进一步的操作。

**2. 如何进行对象克隆**

常用的就是覆盖Object类的clone方法，步骤：

1. 实现Cloneable接口（只是一个标记接口，本身没有任何方法）
2. 覆盖clone()方法，并将可访问性变为public(object的clone为projected，子的可访问级别不能低于父)

来看两个例子：

![img](D:\有道云笔记\924661867@qq.com\64019371311d4b2b9b8dbb524a8e67f8\clipboard.png)

```
public static void main(String[] args) {        Person person1 = new Person();        person1.setName("张三");        person1.setAge(18);         Person person2 = (Person) person1.clone();        System.out.println(person1.toString());        System.out.println(person2.toString());         person2.setName("李四");        System.out.println(person1.toString());        System.out.println(person2.toString());    }
```

输出：

![img](D:\有道云笔记\924661867@qq.com\101a559fe2954cd4ba3072a8d3f92402\clipboard.png)

可以看出使用clone成功的拷贝了person1的信息，且改变person2 的信息不回联动person1的信息，但是若我们在person中添加一个引用变量属性。

![img](D:\有道云笔记\924661867@qq.com\2a0e70bd803f4ef690dffe7ffc6946dd\clipboard.png)

此时的输出为

![img](D:\有道云笔记\924661867@qq.com\a98e2f2da75744878f1b93ff7e74e7cc\clipboard.png)

可以看出，当修改了person2的address，person1的address信息也改变了。

以上就是拷贝里较重要的一个概念：浅拷贝，的典型用例。

**浅拷贝、深拷贝**

浅拷贝：新对象与旧对象的基本数据类型(值类型)一样，但是新旧对象的引用类型都指向以前的对象

深拷贝：在浅拷贝的基础上对引用类型的成员变量也进行了复制。

浅拷贝：

![img](D:\有道云笔记\924661867@qq.com\0a8ca843aabe4e1598b9c2b889f348c3\clipboard.png)

深拷贝：

![img](D:\有道云笔记\924661867@qq.com\afc2a06abdad4e5c9d8fbedab9368891\clipboard.png)

![img](D:\有道云笔记\924661867@qq.com\1f566ee1527b493b9d81dae946b6a10b\clipboard.png)

深拷贝例子：将Person类的clone进行变化

![img](D:\有道云笔记\924661867@qq.com\fa90bbdddb1943cc83854e8f515a2247\clipboard.png)

任然执行之前的clone代码，输出结果：

![img](D:\有道云笔记\924661867@qq.com\8010d4eb29b94843904e95c1eba88415\clipboard.png)

可以看出，如果一个被复制的属性都是基本类型，那么只需要实现当前类的cloneable机制就可以了，此为浅拷贝。

如果被复制对象的属性包含其他实体类对象引用，那么这些实体类对象都需要实现cloneable接口并覆盖clone()方法。

再进一步想，若设计的类有N个对象，有M层继承关系，那么你需要覆盖多少个clone，就会十分麻烦，对代码侵入性很大。

**利用序列化-反序列化实现深拷贝**

把对象写到流里的过程是序列化过程（Serialization），而把对象从流中读出来的过程则叫做反序列化过程（Deserialization）。

```
public class demo {
    public static void main(String[] args) {

        Address address = new Address();
        address.setCountry("China");
        address.setCity("beijing");

        Person person1 = new Person();
        person1.setName("张三");
        person1.setAge(18);
        person1.setAddress(address);

        System.out.println(person1);
        String s1 = JSON.toJSONString(person1);
        System.out.println(s1);
        Person p2 = JSON.parseObject(s1, Person.class);
        System.out.println(p2.toString());
    }
}
```

![img](D:\有道云笔记\924661867@qq.com\ddf99b1844334be2a9f5e53824e8ae48\clipboard.png)