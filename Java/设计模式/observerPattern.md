# 观察者模式
```
/***
 * 抽象被观察者接口
 * 声明了添加、删除、通知观察者方法
 * @author Carl
 *
 */
public interface Serverable {
    
    public void registerObserver(Observer o);
    public void removeObserver(Observer o);
    public void notifyObserver();
    
}
```
```
/**
 * 抽象观察者
 * 定义了一个update()方法，当被观察者调用notifyObservers()方法时，观察者的update()方法会被回调。
 * @author Carl
 *
 */
interface Observer {
    public void update(String message);
}
```

```
/**
 * 被观察者，也就是微信公众号服务
 * 实现了Observerable接口，对Observerable接口的三个方法进行了具体实现
 * @author Carl
 *
 */
class WechatServer implements Serverable {
    
    //注意到这个List集合的泛型参数为Observer接口，设计原则：面向接口编程而不是面向实现编程
    private List<Observer> list;
    private String message;
    
    public WechatServer() {
        list = new ArrayList<Observer>();
    }
    
    @Override
    public void registerObserver(Observer o) {
        
        list.add(o);
    }
    
    @Override
    public void removeObserver(Observer o) {
        if(!list.isEmpty())
            list.remove(o);
    }

    //遍历
    @Override
    public void notifyObserver() {
        for(int i = 0; i < list.size(); i++) {
            Observer oserver = list.get(i);
            oserver.update(message);
        }
    }
    
    public void setInfomation(String s) {
        this.message = s;
        System.out.println("微信服务更新消息： " + s);
        //消息更新，通知所有观察者
        notifyObserver();
    }

}
```

```
/**
 * 观察者
 * 实现了update方法
 * @author Carl
 *
 */
class User implements Observer {

    private String name;
    private String message;
    
    public User(String name) {
        this.name = name;
    }
    
    @Override
    public void update(String message) {
        this.message = message;
        read();
    }
    
    public void read() {
        System.out.println(name + " 收到推送消息： " + message);
    }
    
}
```

```
/**
 * 测试
 * @author Carl
 * 	简介
 * 		在对象之间定义了一对多的依赖，这样一来，当一个对象改变状态，依赖它的对象会收到通知并自动更新。
 *	角色
 *		抽象被观察者角色：也就是一个抽象主题，它把所有对观察者对象的引用保存在一个集合中，
 *		每个主题都可以有任意数量的观察者。抽象主题提供一个接口，可以增加和删除观察者角色。一般用一个抽象类和接口来实现。
 *		抽象观察者角色：为所有的具体观察者定义一个接口，在得到主题通知时更新自己。
 *		具体被观察者角色：也就是一个具体的主题，在集体主题的内部状态改变时，所有登记过的观察者发出通知。
 *		具体观察者角色：实现抽象观察者角色所需要的更新接口，一边使本身的状态与制图的状态相协调。
 */
public class Test {

	public static void main(String[] args) {
		//创建
		 WechatServer server = new WechatServer();
	        
            Observer userZhang = new User("ZhangSan");
            Observer userLi = new User("LiSi");
            Observer userWang = new User("WangWu");
            
            server.registerObserver(userZhang);
            server.registerObserver(userLi);
            server.registerObserver(userWang);
            server.setInfomation("谁说PHP是世界上最好用的语言！");
            
            System.out.println("----------------------------------------------");
            server.removeObserver(userZhang);
            server.setInfomation("JS才是世界上最好用的语言！");
	        
	}

}

```