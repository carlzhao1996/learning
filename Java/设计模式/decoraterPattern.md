# 装饰者模式
```java
/**
* 抽象构件角色
*/
public  abstract class Component {
	public abstract void operation();
}

/**
* 具体构件角色
*/
class ConcreteComponent extends Component {		
	@Override
	public void operation() {
		System.out.println("具体对象的操作");
	}
}
/**
 * 装饰角色
 */
abstract class Decorator extends Component {
	protected Component component;	
	public void setComponent(Component component){
		this.component = component;
	}
	
	@Override
	public void operation() {
		if(component != null){
			component.operation();
		}
	}
}


 
/**
 * 具体装饰角色A
 */
class ConcreteDecoratorA extends Decorator {
	//本类的独有功能
	private String addedState;
	//首先运行原Component的operation()，再执行本类的功能，相当于对原Component进行了装饰
	@Override
	public void operation(){
		super.operation();
		addedState = "New State";
		System.out.println("具体装饰对象A的操作。" + addedState);
	}
}

class ConcreteDecoratorB extends Decorator {
	//首先运行原Component的operation()，再执行本类的功能，相当于对原Component进行了装饰
	@Override
	public void operation() {
		super.operation();
		AddedBehavior();
		System.out.println("具体装饰对象B的操作");
	}
	//本类的独有功能
	private void AddedBehavior() {
		System.out.println("我是具体装饰对象B的独有方法");		
	}
}

/**
 *“Person”类（ConcreteComponent） 
 */
class Person {
	 
	public Person() {		
	}
 
	private String name;
	public Person(String name){
		this.name = name;
	}
	
	public void show(){
		System.out.println(String.format("装扮的%s", name));
	}
}

/**
 * 服饰类（Decorator）
 */
class Finery extends Person {
	protected Person component;
	public void decorate(Person component) {
		this.component = component;
	}
	@Override
	public void show() {
		if(component != null){
			component.show();
		}
	}
}

/**
 * 具体服饰类（ConcreteDecorator）
 */
class TShirts extends Finery {
	@Override
	public void show() {
		System.out.println("大T恤");
		super.show();
	}
}



/**
 * 具体服饰类（ConcreteDecorator）
 */
class Sneakers extends Finery {
	@Override
	public void show() {
		System.out.println("球鞋");
		super.show();
	}
}
 
 
/**
 * 具体服饰类（ConcreteDecorator）
 */
class BigTrouser extends Finery {
	@Override
	public void show() {
		System.out.println("垮裤");
		super.show();
	}
}
```

```java
public static void main(String[] args) {
    ConcreteComponent c = new ConcreteComponent();
    ConcreteDecoratorA d1 = new ConcreteDecoratorA();
    ConcreteDecoratorB d2 = new ConcreteDecoratorB();
    
    d1.setComponent(c);
    d2.setComponent(d1);
    d2.operation();
    /*运行输出结果：
    具体对象的操作
    具体装饰对象A的操作。New State
    我是具体装饰对象B的独有方法
    具体装饰对象B的操作
    */
    
    Person xc = new Person("小菜");
    
    Sneakers pqx = new Sneakers();
    BigTrouser kk = new BigTrouser();
    TShirts dtx = new TShirts();
    
    pqx.decorate(xc);		
    kk.decorate(pqx);
    dtx.decorate(kk);
    dtx.show();
    /*
    运行结果：
    大T恤
    垮裤
    球鞋
    装扮的小菜
    */

}
	

}
```