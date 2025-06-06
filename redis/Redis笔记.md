# Redis缓存

## 1.缓存更新策略

### 1.删除缓存还是更新缓存?

- 更新缓存:每次更新数据库都更新缓存,无效写操作较多

- **删除缓存:更新数据库时让缓存失效,查询时再更新缓存**   

  > [!CAUTION]
  >
  > 企业一般采用该策略

  

### 2.如何保证缓存与数据库的操作的同时成功或失败?

- 单体系统,将缓存与数据库操作放在一个事务 

  **可以使用transactional注解事务**

- 分布式系统,利用TCC等分布式事务方案

### 3.先操作缓存还是先操作数据库?

- 先删除缓存,再操作数据库
  先操作数据库,再删除缓存     ****

  > [!IMPORTANT]
  >
  > **<u>先操作数据库,再删除缓存</u>**

  

- ![image-20250525182229821](typora-user-images\image-20250525182229821.png)

- ![image-20250525182305362](typora-user-images\image-20250525182305362.png)

- 

  > [!IMPORTANT]
  >
  > **先操作数据库,后操作缓存切记设置超时时间作为兜底**

## 2.缓存穿透

### 2.1**基于缓存空对象解决缓存击穿问题**

- ![image-20250525184858358](typora-user-images\image-20250525184858358.png)



### **2.2缓存穿透产生的原因是什么?**

用户请求的数据在缓存中和数据库中都不存在,不断发起这样的请求

给数据库带来巨大压力

### **2.3缓存穿透的解决方案有哪些?**

- 缓存null值
- 布隆过滤

## **3.缓存雪崩问题**



**缓存雪崩:**是指在同一时段大量的缓存key同时失效或者Redis服务宕机,导致大量请求到达数据库,带来巨大压力。

![image-20250525191935476](typora-user-images\image-20250525191935476.png)

### 3.1缓存雪崩的解决



## 4.缓存击穿

缓存击穿问题也叫热点Key问题,就是一个被高并发访问并且缓存重建业务较复杂的key突然失效了,无数的请求访问会在瞬间给数据库带来巨大的冲击。

### 4.1击穿

![image-20250525194837278](typora-user-images\image-20250525194837278.png)

### 4.2互斥锁和逻辑过期

![image-20250525194917385](typora-user-images\image-20250525194917385.png)

### 4.3方案优缺点

![image-20250525194927275](typora-user-images\image-20250525194927275.png)