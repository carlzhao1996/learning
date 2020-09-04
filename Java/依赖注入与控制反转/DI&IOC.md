# Java 依赖注入和控制反转
IOC是为了减少代码的耦合度。
## IOC
### 简单IOC实现
e.g.
UserService Interface
```java
public interface UserService {
	/**
	 * 保存用户信息
	 */
	public void save(User user);
}
```

UserDao
```java
public interface UserDao {
	/**
	 * 保存用户的方法
	 */
	public void save(User user);
}
```

保存User接口实现类
```java
public class UserDaoImpl implements UserDao{

	@Override
	public void save(User user) {
		// TODO Auto-generated method stub
		System.out.println("保存用户信息到数据库");
	}

}
```
```java
public class UserDaoOracleImpl implements UserDao{

	@Override
	public void save(User user) {
		// TODO Auto-generated method stub
		System.out.println("保存到Oracle数据库了");
	}

}
```

服务接口实现类
```java
public class UserServiceImpl implements UserService{
	//原来的形式
	/*private UserDao userDao=new UserDaoImpl();*/
	//引入userdao,userServiceImpl依赖userDao,实例化所依赖的UserDao
	private UserDao userDao=UserDaoFactory.getInstance();
	@Override
	public void save(User user) {
		userDao.save(user);
	}
}
```

工厂类：工厂类负责管理dao实例
```java
public class UserDaoFactory {
	//负责创建用户Dao实例的方法
	public static UserDao getInstance(){
		return new UserDaoOracleImpl();
	}
}
```

Main
```java
public class UserServiceImplTest {

	@Test
	public void testUserServiceImpl(){
		UserService userService=new UserServiceImpl();
		User user=new User();
		userService.save(user);
	}
}
```



