title: java代理
author: 大帅
date: 2018-06-27 16:36:04
tags:
---
创建动态代理
 
- 利用Java的Proxy类，调用Proxy.newProxyInstance()，创建动态对象十分简单。
	
      InvocationHandler handler = new MyInvocationHandler(...);
      Class proxyClass = Proxy.getProxyClass(Foo.class.getClassLoader(), new Class[] { Foo.class });
      Foo f = (Foo) proxyClass.getConstructor(new Class[] { InvocationHandler.class }).newInstance(new Object[] { handler });
         //或
         Foo f = (Foo) Proxy.newProxyInstance(Foo.class.getClassLoader(),
                                     new Class[] { Foo.class },
                                     handler);
                                     
                                     
案例：

	public interface IVehical {

    void run();
    
	}

 	//concrete implementation
	public class Car implements IVehical{

    public void run() {
    System.out.println("Car is running");
    }

}

	//proxy class
					
    public class VehicalProxy {
    private IVehical vehical;

    public VehicalProxy(IVehical vehical) {
    this.vehical = vehical;
    }

    public IVehical create(){
    final Class<?>[] interfaces = new Class[]{IVehical.class};
    final VehicalInvacationHandler handler = new VehicalInvacationHandler(vehical);
    
    return (IVehical) Proxy.newProxyInstance(IVehical.class.getClassLoader(), interfaces, handler);
    }
    
    public class VehicalInvacationHandler implements InvocationHandler{

    private final IVehical vehical;
    
    public VehicalInvacationHandler(IVehical vehical) {
        this.vehical = vehical;
    }

    public Object invoke(Object proxy, Method method, Object[] args)
        throws Throwable {

        System.out.println("--before running...");
        Object ret = method.invoke(vehical, args);
        System.out.println("--after running...");
        
        return ret;
    }
    
    }
	}

	public class Main {
    public static void main(String[] args) {
    
    IVehical car = new Car();
    VehicalProxy proxy = new VehicalProxy(car);
    
    IVehical proxyObj = proxy.create();
    proxyObj.run();
    }
}