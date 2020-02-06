# 工厂模式

## 工厂方法模式的介绍  
* 工厂方法模式定义一个用于创建对象的接口，让子类决定实例化哪一个类。Factory Method是一个类的实例化延迟到其子类。
*  在工厂方法模式中，核心的工厂类不再负责所有的产品的创建，而是将具体创建的工作交给子类去做。这个核心类则摇身一变成为了一个抽象工厂角色，仅负责给出具体工厂子类必须实现的接口，而不接触哪一个产品类应当被实例化这种细节。

## 工厂方法模式角色
* 抽象工厂（Creator）角色：担任这个角色的是工厂方法模式的核心，它是与应用程序无关的。任何在模式中创建对象的工厂类必须实现这个接口。在实际的系统中，这个角色也常常使用抽象Java 类实现。
* 具体工厂（Concrete Creator）角色：担任这个角色的是实现了抽象工厂接口的具体Java 类。具体工厂角色含有与应用密切相关的逻辑，并且受到应用程序的调用以创建产品对象。在本系统中给出了两个这样的角色，也就是具体Java 类ConcreteCreator1 和ConcreteCreator2。
* 抽象产品（Product）角色：工厂方法模式所创建的对象的超类型，也就是产品对象的共同父类或共同拥有的接口。
* 具体产品（Concrete Product）角色：这个角色实现了抽象产品角色所声明的接口。工厂方法模式所创建的每一个对象都是某个具体产品角色的实例。  

## 工厂模式的优缺点
### 优点
* 在工厂方法模式中，工厂方法用来创建客户所需要的产品，同时还向客户隐藏了哪种具体产品类将被实例化这一细节，用户只需要关心所需产品对应的工厂，无需关心创建细节，甚至无需知道具体产品类的类名。
* 基于工厂角色和产品角色的多态性设计是工厂方法模式的关键。它能够使工厂可以自主确定创建何种产品对象，而如何创建这个对象的细节则完全封装在具体工厂内部。工厂方法模式之所以又被称为多态工厂模式，正是因为所有的具体工厂类都具有同一抽象父类。  
* 使用工厂方法模式的另一个优点是在系统中加入新产品时，无需修改抽象工厂和抽象产品提供的接口，无需修改客户端，也无需修改其他的具体工厂和具体产品，而只要添加一个具体工厂和具体产品就可以了，这样，系统的可扩展性也就变得非常好，完全符合“开闭原则”。
### 缺点
* 在添加新产品时，需要编写新的具体产品类，而且还要提供与之对应的具体工厂类，系统中类的个数将成对增加，在一定程度上增加了系统的复杂度，有更多的类需要编译和运行，会给系统带来一些额外的开销。
* 由于考虑到系统的可扩展性，需要引入抽象层，在客户端代码中均使用抽象层进行定义，增加了系统的抽象性和理解难度，且在实现时可能需要用到DOM、反射等技术，增加了系统的实现难度。

### 适用环境
* 一个类不知道它所需要的对象的类：在工厂方法模式中，客户端不需要知道具体产品类的类名，只需要知道所对应的工厂即可，具体的产品对象由具体工厂类创建；客户端需要知道创建具体产品的工厂类。 
* 一个类通过其子类来指定创建哪个对象：在工厂方法模式中，对于抽象工厂类只需要提供一个创建产品的接口，而由其子类来确定具体要创建的对象，利用面向对象的多态性和里氏代换原则，在程序运行时，子类对象将覆盖父类对象，从而使得系统更容易扩展。
* 将创建对象的任务委托给多个工厂子类中的某一个，客户端在使用时可以无需关心是哪一个工厂子类创建产品子类，需要时再动态指定，可将具体工厂类的类名存储在配置文件或数据库中。 

e.g.
```
//抽象产品   
public abstract class PenCore{  
	String color;  
	public abstract void writeWord(String s);  
}  
//具体产品  
class RedPenCore extends PenCore {  
    RedPenCore() {  
        color = "红色";  
    }  
    public void writeWord(String s) {  
        System.out.println("写出" + color + "的字" + s);  
    }  
}
//具体产品  
class BluePenCore extends PenCore {  
    BluePenCore() {  
        color = "蓝色";  
    }  
    public void writeWord(String s) {  
        System.out.println("写出" + color + "的字" + s);  
    }  
}  
//具体产品  
class BlackPenCore extends PenCore {  
    BlackPenCore() {  
        color = "黑色";  
    }  
    public void writeWord(String s) {  
        System.out.println("写出" + color + "的字" + s);  
    }  
}
//具体产品
class GreenPenCore extends PenCore {  
	GreenPenCore() {  
        color = "绿色";  
    }  
    public void writeWord(String s) {  
        System.out.println("写出" + color + "的字" + s);  
    }  
}  

//构造者  
abstract class BallPen {  
    BallPen(){  
        System.out.println("生产一只装有"+getPenCore().color+"笔芯的圆珠笔");  
    }  
    public abstract PenCore getPenCore();  
}  
class RedBallPen extends BallPen {  
    public PenCore getPenCore() {  
        return new RedPenCore();  
    }  
} 
//具体构造者  
class BlueBallPen extends BallPen {  
    public PenCore getPenCore() {  
        return new BluePenCore();  
    }  
}  
//具体构造者  
class BlackBallPen extends BallPen {  
    public PenCore getPenCore() {  
        return new BlackPenCore();  
    }  
}  
//具体构造者  
class GreenBallPen extends BallPen {  
    public PenCore getPenCore() {  
        return new GreenPenCore();  
    }  
}  
```

```
public class Test {

	public static void main(String[] args) {
		//可以有什么的字体
		BallPen bpl=new RedBallPen();
		BallPen bpr=new BlueBallPen();
		BallPen bpb=new BlackBallPen();
		BallPen bpls=new GreenBallPen();
	}

}
```