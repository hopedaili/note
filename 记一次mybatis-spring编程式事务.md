## 记一次mybatis-spring编程式事务

单位的一个项目，声明式事务怎么配置都不好用，注解和aop都是，只能临时用编程式事务了

### 1.代码实现 

```java
@RestController
@RequestMapping("/userInfo")
public class UserController {
    
    @Autowired
	private UserGunMapper userGunMapper;
	@Autowired
	private OdOrderMapper odOrderMapper;
	private PlatformTransactionManager txManager;
	
	@RequestMapping("/tx")
	public int testTx(){
		DefaultTransactionDefinition def = new DefaultTransactionDefinition();
		def.setIsolationLevel(TransactionDefinition.ISOLATION_READ_COMMITTED);
		def.setPropagationBehavior(TransactionDefinition.PROPAGATION_REQUIRED);
		//事务状态类，通过PlatformTransactionManager的getTransaction方法根据事务定义获取；获取事务状态后，Spring根据传播行为来决定如何开启事务
		TransactionStatus status = txManager.getTransaction(def);
		try {
			OdOrder odOrder = new OdOrder();
			odOrder.setTradeFlowNo("111111");
			odOrderMapper.insertSelective(odOrder);
			int i = 2/0;
			UserGun userGun = new UserGun();
			userGun.setGunNo("111111");
			userGunMapper.insertSelective(userGun);
			txManager.commit(status);
		}catch (Exception e){
			txManager.rollback(status);
		}
		return 0;
	}
}
```

### 2.封装类（好用，但不知道会不会由于单例，导致所有方法的事务一起回滚）

```java
package com.kd.yxcd.util;

import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.TransactionDefinition;
import org.springframework.transaction.TransactionStatus;
import org.springframework.transaction.support.DefaultTransactionDefinition;

/**
 * @author daili
 * @create 2020-07-06 10:16
 */
public class TxUtil {

    public static TxUtil me;

    private TxUtil(){
        //单例
    }

    //双重锁
    public static TxUtil getInstance(){
        if(me == null){
            synchronized (TxUtil.class){
                if(me == null){
                    me = new TxUtil();
                }
            }
        }
        return me;
    }

    private static PlatformTransactionManager txManager;

    private static DefaultTransactionDefinition def;

    //事务状态类，通过PlatformTransactionManager的getTransaction方法根据事务定义获取；获取事务状态后，Spring根据传播行为来决定如何开启事务
    private static TransactionStatus status;

    public void begin(){
        //def = new DefaultTransactionDefinition();
        def.setIsolationLevel(TransactionDefinition.ISOLATION_READ_COMMITTED);
        def.setPropagationBehavior(TransactionDefinition.PROPAGATION_REQUIRED);
        status = txManager.getTransaction(def);
    }

    public void commit(){
        txManager.commit(status);
    }

    public void rollback(){
        txManager.rollback(status);
    }

}

```

