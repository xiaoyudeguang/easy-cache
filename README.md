# Github文档已不再维护，请移步码云https://gitee.com/xiaoyudeguang/projects 查看相关项目
# easy-cache

## 介绍
一个极度便捷的内存型缓存，操作简单，操作简单，操作简单，重要的事情说三遍。你想要的缓存功能都不支持，只有添加缓存这一功能，用于缓存一些万年不变的配置项再好不过。对你的系统来说，有它没它基本没啥影响。更坑爹的是，删除缓存值，只能在方法中手动删除。。。再烂的产品也总会有合适的适用场景，没错，就是这么自信。码云地址：https://gitee.com/xiaoyudeguang/easy-cache

PS：写这个工具是有自己的场景的，一大堆的物联网设备要求连接服务器，每次连接还都得验证一下是不是合法设备，是不是已经注册，数据库里会存放她们的设备号。不想引入第三方缓存，太重了，没必要；不想每次都去查询数据库，因为连接数量实在太大了，数据库得炸了；可是又不能都写在业务里去做判断吧？那罗里吧嗦的代码不得烦死？怎么办呢？自己搞一个缓存呗，就基于内存，于是easy-cache就诞生了。

整体思路就是：定义一个注解，切面切了它，然后根据方法名和方法参数中的某个参数的值作为key来缓存方法执行结果到内存，下次执行如果发现已经有值了，就不去执行方法了，直接返回内存中的值。

## 使用说明
##### 添加缓存：直接在方法上声明@EasyCache注解
```
@EasyCache(key = "name", todo = "")
public int doService(String name) {
    return this.hascode();
}
```
> 请注意，你的方法参数中一定得有一个参数的参数名和@EasyCache注解的key值一样，不然缓存是不会生效的。那个参数可以直接存在于方法参数中(位置无所谓)，也可以存在于某个map(Map实现类)中，或者，某个实例化的EasyMap(HashMap的增强版，本质上还是Map)对象中。

#### 删除缓存
本来，我的业务室不需要删除缓存的。想了想，加上吧，万一有人需要呢。
1. 在需要删除缓存对的方法上加上下面的注解(你可能已经猜到@EasyCache注解的value参数有默认值了。。。)
```
@EasyCache(value = Operation.REMOVE, key = "name", todo = "")
public int delete(String name) {
    return this.hascode();
}
```
2. 比如很奇葩的业务哈，直接调用定义在抽象层或者三方包的方法，没有修改类方法的实现，咋办？不管你用哪个缓存，都会遇到这个问题是吧？请看：

```
CacheUtils.dele(key);
```
额。。。没错，就是在你的代码里加上这句蹩脚的代码。。。

## Maven引用(如果版本有变化，请自行去maven中央仓库引用)
```
<dependency>
    <groupId>io.github.xiaoyudeguang</groupId>
    <artifactId>easy-cache</artifactId>
    <version>3.0.2-RELEASE</version>
</dependency>
```
