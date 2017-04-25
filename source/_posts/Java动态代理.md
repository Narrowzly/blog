---
title: Java动态代理
categories: 设计模式
tags: [设计模式, Java]
---
### 前言（可以直接跳过）
> 好久没更新博客，最近都忙着和赵志文打王者荣耀，然后就是敲代码了。动态代理这玩意昨天看到这个名词，就想着什么鬼玩意。上网查了下是一种设计模式，就没有细究了。然而今天刚学到Spring AOP 这一部分，瞬间懵比，怎么完全听不懂马老师在讲什么玩意。为此我必须好好敏感下这玩意，通过40分钟的刷知乎大概明白了怎么回事。

### 正文
> 假设我们有一个业务逻辑那就是在某个类的每个方法运行的时候前后添加开始和结束的日志。我们会怎么办？首先我们看下代码示例首先我们定义一个Person类的接口
> ```  
>	public interface Person {
        public void eat();
        public void drink();
        public void sleep();
        public void WC();
    }
```
> 然后是一个学生类的具体实现
> ``` 
>  public class Student implements Person {
	@Override
	public void eat() {
		System.out.println("I am eating!");
	}
	@Override
	public void drink() {
		System.out.println("I am drinking!");
	}
	@Override
	public void sleep() {
		System.out.println("I am sleeping!");
	}
	@Override
	public void WC() {
		System.out.println("I am WCing!");
	}
}
```
> 我们就是要在Student这个类的各个方法添加逻辑
1. 很简单就是直接在每个原方法上添加代码，很累，假设我给你500个方法你自己去添加吧。再者就是可能我不想改Student这个类了，我要改Teacher这个类，那么很麻烦要把Student这个类要还原，Teacher类又要重新添加。GG
2. 静态代理的思想就是， 看下面的代码
```
public class StaticProxy implements Person {
	private Person s = new Student();
	@Override
	public void eat() {
		s.eat();
	}
	@Override
	public void drink() {
		s.drink();
	}
	@Override
	public void sleep() {
		s.sleep();
	}
	@Override
	public void WC() {
		s.WC();
	}
}
```
>很简单实现Person接口然后再持有Student的引用，再添加。有效解决了方法一的第二种情况，可是添加500个方法我表示依然蛋疼。这时候就要出现我们的动态代理了
3. 动态代理的解决方案
``` 
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
public class DynamicProxy implements InvocationHandler {
	Object target;
	public Object bind(Object target) {
		this.target = target;
		return Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(), this);
	}
	@Override
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		System.out.println("---Start---");
		Object result = method.invoke(target, args);
		System.out.println("---End---");
		return result;
	}
}
```
```
public class Test {
	public static void main(String[] args) {
		DynamicProxy proxy = new DynamicProxy();
		Person s = (Person)proxy.bind(new Student());
		s.drink();
		s.sleep();
		s.eat();
		s.WC();
	}
} 
 ```
> 下面我来依照我自身的理解来说，首先为什么要实现InvocationHandler这个接口，因为从接口名字可以理解调用处理者，那么就是说我调用代理类的方法我肯定先调用invoke方法去执行，而不像一开始静态代理里面那样这些都是写死的，那么bind方法是干嘛呢其实就是返回一个动态的代理类，这个代理类是根据传入的对象和实现InvocationHandler这个接口的生成的。关于具体怎么实现抽个空研究下吧