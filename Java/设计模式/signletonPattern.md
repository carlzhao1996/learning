# 单例模式

## 饿汉模式
饿汉式在类创建的同时就已经创建好一个静态的对象供系统使用，以后不再改变，所以天生是线程安全的。
```java
public class SingEHan {
	 private SingEHan() {}
	    private static final SingEHan single = new SingEHan();
	    //静态工厂方法 
	    public static SingEHan getInstance() {
	        return single;
	    }
}
```

## 懒汉模式
懒汉模式是单例类.在第一次调用的时候实例化自己 
```java
public class Singleton {
  private Singleton() {}
  private static Singleton single=null;
  //静态工厂方法 
  public static Singleton getInstance() {
       if (single == null) {  
           single = new Singleton();
       }  
      return single;
  }
}

//这种方式( 线程安全)
class Singleton1 {  
    private static class LazyHolder {  
       private static final Singleton1 INSTANCE = new Singleton1();  
    }  
    private Singleton1 (){}  
    public static final Singleton1 getInstance() {  
       return LazyHolder.INSTANCE;  
    }  
}  
```