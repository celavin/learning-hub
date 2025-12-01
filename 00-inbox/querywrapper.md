# mybatisplus初上手
mapper类继承了basemapper后,简单sql语句不用自己写,直接用内置的

内置方法没有getUsername(具体某个参数),怎么解决如下代码
``` java
    public User getUserByUsername(String userName){
        // 翻译：SELECT * FROM users WHERE username = ?
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.eq("username", userName); // eq 表示 equal (等于)

        return userMapper.selectOne(wrapper);
    }
```

## 了解条件构造器 (Wrapper)
以前写 SQL，要手动拼写字符串："WHERE username = " + name。 现在有 MP，我们是用“Java 对象的方式”来组装 SQL。

### 第一步,创建条件盒子
``` java
QueryWrapper<User> wrapper = new QueryWrapper<>();
```

### 第二步,wrapper里放条件
``` java
wrapper.eq("username", userName); // eq 表示 equal (等于)
```
eq 是什么意思？ 它是 Equal（等于）的缩写。MP 还有 .ne (不等于), .gt (大于), .like (模糊查询) 等等。  
"username"是数据库的字段名,userName则是java的变量  
执行这一步,相当于sql语句的WHERE username = userName  

### 第三步,用内置的selectOne方法,参数wrapper就是条件
``` java
return userMapper.selectOne(wrapper);
```

## 进阶的LambdaQueryWrapper
防止手抖写错的lambda升级版,实际更常用这个
``` java
    // 1. 创建 Lambda 包装器
    LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();

    // 2. 注意这里！不再写 "userName" 字符串，而是用 Java 方法引用
    // 意思是：去 User 类里找 getUsername 对应的那个数据库字段，让它等于 username 变量
    wrapper.eq(User::getUsername, inputName);

    // 3. 查询
    return userMapper.selectOne(wrapper);
```

>wrapper.eq(User::getUsername, inputName);  
详细解释一下这一句,有点绕  
左边告诉mybatisplus,让它查一下User类里的username属性对应数据库的哪一列  
之前我在User类里加了@TableName的注解,所以它能找到映射关系  
这一步相当于 WHERE username = ?
右边则是方法传进来的变量
相当于补全了问号
