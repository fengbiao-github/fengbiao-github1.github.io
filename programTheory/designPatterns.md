# 设计模式
## 1、七大设计原则

## 2、创建型设计模式

### 2.1、单例模式

#### 2.1.1、作用

#### 2.1.2、使用场景

#### 2.1.3、五种实现方式及优缺点

* **饿汉式**：线程安全；调用效率高；不能延时加载，类初始化时，立即加载

```
public class Singleton1 {

    private static Singleton1 INSTANCE = new Singleton1();

    private Singleton1() {
    }

    public static Singleton1 getInstance() {
        return INSTANCE;
    }

}
```

* **懒汉式**：线程安全；调用效率不高；可以延时加载  

```
public class Singleton2 {

    private static Singleton2 INSTANCE;

    private Singleton2() {
    }

    public static synchronized Singleton2 getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new Singleton2();
        }
        return INSTANCE;
    }
}
```

* **双重检查**：由于JVM底层内部模型原因，可能出现问题，需要加volatile修饰符，**避免指令重排序**  

````JAVA
public class Singleton3 {

    private static volatile Singleton3 INSTANCE; // 防止指令重排

    private Singleton3() {
    }

    public static Singleton3 getInstance() {
        if (INSTANCE == null) {
            synchronized (Singleton3.class) {
                if (INSTANCE == null) {
                    INSTANCE = new Singleton3();
                }
            }
        }
        return INSTANCE;
    }
}
````

* **静态内部类**：线程安全；调用效率高；可以延时加载  

````JAVA
public class Singleton4 {

    private Singleton4() {
    }

    public static class Inner { // 内部类只有被调用时，才会被加载
        public static Singleton4 INSTANCE = new Singleton4();
    }

    public static Singleton4 getInstance() {
        return Inner.INSTANCE;
    }
}
````

* **枚举类**：线程安全；调用效率高；不能延时加载，且**不可以通过反射和反序列化来破解**  

````JAVA
public enum Singleton5 {

    INSTANCE;
}
````

#### 2.1.4、破解单例模式
* **序列化**(要求实现序列化接口)

````JAVA
// 破解单例模式
@Test
public void test7() throws Exception {
    Singleton1 singleton11 = Singleton1.getInstance();
    ByteArrayOutputStream baos = new ByteArrayOutputStream();
    ObjectOutputStream oos = new ObjectOutputStream(baos);
    oos.writeObject(singleton11);
    ByteArrayInputStream bais = new ByteArrayInputStream(baos.toByteArray());
    ObjectInputStream ois = new ObjectInputStream(bais);
    Singleton1 singleton12 = (Singleton1) ois.readObject();
    System.out.println(singleton11);
    System.out.println(singleton12);
    System.out.println(singleton11 == singleton12);
}
// 解决方案：在单例模式类中，增加反序列化回调方法private Object readResolve() throws ObjectStreamException，返回实例对象
public class Singleton1 implements Serializable {

    /**
     * 
     */
    private static final long serialVersionUID = 1L;

    private static Singleton1 INSTANCE = new Singleton1();

    private Singleton1() {
    }

    public static Singleton1 getInstance() {
        return INSTANCE;
    }

    private Object readResolve() throws ObjectStreamException {
        return INSTANCE;
    }

}
````

* **反射**  

````JAVA
// 破解单例模式
@Test
public void test6() throws Exception {
    Singleton1 singleton11 = Singleton1.getInstance();
    Constructor<Singleton1> constructor = Singleton1.class.getDeclaredConstructor(null);
    constructor.setAccessible(true);
    Singleton1 singleton12 = constructor.newInstance(null);
    System.out.println(singleton11 == singleton12);
}
// 解决方案：在单例模式类的私有构造方法中，判断实例是否为null，如果非null，则抛出异常
public class Singleton1 {

    private static Singleton1 INSTANCE = new Singleton1();

    private Singleton1() {
        if(INSTANCE != null) {
            throw new RuntimeException();
        }
    }

    public static Singleton1 getInstance() {
        return INSTANCE;
    }

}
````

## 3、结构型设计模式
## 4、行为型设计模式
## 5、UML
