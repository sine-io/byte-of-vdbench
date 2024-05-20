# 附录

## 源码解析

### File system

`FwgThread`类继承自 `Java`原生`Thread`类

`OpWrite`、`OpRead`等操作类继承自`FwgThread`类

`Thread`类调用`start()`方法的结果是 --- 最终调用类中的`run()`方法 --- 详见[链接](https://blog.csdn.net/qq_21383435/article/details/123021752)

```java

```

### forxxx参数源码分析

```java
// For_loop.java
```
