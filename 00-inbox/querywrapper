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
"username"是数据库的字段名,userName则是java的变量
执行这一步,相当于sql语句的WHERE username = userName

### 第三步,用内置的selectOne方法,参数wrapper就是条件
``` java
return userMapper.selectOne(wrapper);
```

## 进阶的LambdaQueryWrapper
防止手抖写错
