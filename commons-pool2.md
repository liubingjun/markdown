---
title: commons-pool2
date: 2016-06-01 23:40:40
tags: 工具
---
# PooledObjectFactory
池对象工厂(PooledObjectFactory接口):用来创建池对象, 将不用的池对象进行钝化(passivateObject), 对要使用的池对象进行激活(activeObject), 对池对象进行验证(validateObject), 对有问题的池对象进行销毁(destroyObject)等工作

PooledObjectFactory是一个池化对象工厂接口，定义了生成对象、激活对象、钝化对象、销毁对象的方法，如下：
```java
public interface PooledObjectFactory<T> {
    PooledObject<T> makeObject() throws Exception;

    void destroyObject(PooledObject<T> var1) throws Exception;

    boolean validateObject(PooledObject<T> var1);

    void activateObject(PooledObject<T> var1) throws Exception;

    void passivateObject(PooledObject<T> var1) throws Exception;
}
```