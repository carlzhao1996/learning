# 代理模式
## 静态代理模式
### 优势
可以做到在不修改目标对象的功能前提下,对目标功能扩展.
### 劣势
因为代理对象需要与目标对象实现一样的接口,所以会有很多代理类,类太多.同时,一旦接口增加方法,目标对象与代理对象都要维护.

e.g.
```java
public interface IuserDao {
	
	void save();

}

class UserDao implements IuserDao {
    public void save() {
        System.out.println("----已经保存数据!----");
    }
}

class UserDaoProxy implements IuserDao{
    //接收保存目标对象
    private IuserDao target;
    public UserDaoProxy(IuserDao target){
        this.target=target;
    }

    public void save() {
        System.out.println("开始事务...");
        target.save();//执行目标对象的方法
        System.out.println("提交事务...");
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        //目标对象
        UserDao target = new UserDao();

        //代理对象,把目标对象传给代理对象,建立代理关系
        UserDaoProxy proxy = new UserDaoProxy(target);

        proxy.save();//执行的是代理的方法
    }
	
}
```

## 动态代理模式
动态代理的点
* 代理对象,不需要实现接口
* 代理对象的生成,是利用JDK的API,动态的在内存中构建代理对象(需要我们指定创建代理对象/目标对象实现的接口的类型)
* 动态代理也叫做:JDK代理,接口代理,JDK中生成代理对象的API
* 代理类所在包:java.lang.reflect.Proxy
* JDK实现代理只需要使用newProxyInstance方法,但是该方法需要接收三个参数,完整的写法是:
```java
static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces,InvocationHandler h )
```

注意该方法是在Proxy类中是静态方法,且接收的三个参数依次为:
 *	ClassLoader loader:指定当前目标对象使用类加载器,获取加载器的方法是固定的
 *	Class<?>[] interfaces:目标对象实现的接口的类型,使用泛型方式确认类型
 *	InvocationHandler h:事件处理,执行目标对象的方法时,会触发事件处理器的方法,会把当前执行目标对象的方法作为参数传入

```java
/**
 * 创建动态代理对象
 * 动态代理不需要实现接口,但是需要指定接口类型
 */
class ProxyFactory{

    //维护一个目标对象
    private Object target;
    public ProxyFactory(Object target){
        this.target=target;
    }

   //给目标对象生成代理对象
public Object getProxyInstance(){
    return Proxy.newProxyInstance(
        target.getClass().getClassLoader(),
        target.getClass().getInterfaces(),
        new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println("开始事务2");
                //执行目标对象方法
                Object returnValue = method.invoke(target, args);
                System.out.println("提交事务2");
                return returnValue;
            }
        }
    );
}
}
```

```java
/**
 * 测试类
 * 总结：
 * 	代理对象不需要实现接口,但是目标对象一定要实现接口,否则不能用动态代理
 */
public class Test1 {
    public static void main(String[] args) {
        // 目标对象
        IuserDao target = new UserDao();
        // 【原始的类型 class cn.itcast.b_dynamic.UserDao】
        System.out.println(target.getClass());

        // 给目标对象，创建代理对象
        IuserDao proxy = (IuserDao) new ProxyFactory(target).getProxyInstance();
        // class $Proxy0   内存中动态生成的代理对象
        System.out.println(proxy.getClass());

        // 执行方法   【代理对象】
        proxy.save();
    }
}
```