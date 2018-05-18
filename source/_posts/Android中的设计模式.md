---
title: Android中的设计模式
date: 2018-05-21 16:54:16
tags: "设计模式"
categories: "Android"
---
（1）单例模式：
			简介：保证一个类仅有一个实例，并提供一个访问它的全局访问点。
			示例：Android中的系统级服务都是通过容器的单例模式实现方式，以单例形式存在，减少了资源消耗。
			比如LayoutInflater Service，将这些服务以键值对的形式存储在一个HashMap容器中，用户使用时只需要根据key来获取到对应的ServiceFetcher，然后通过ServcieFetcher对象的getService函数来获取到具体的服务对象，第一次获取时会调用ServcieFetcher的createService函数创建服务对象，然后将该对象缓存到一个列表中，下次再取时直接从缓存中获取，避免重复创建对象，从而达到单例的效果。
（2）抽象工厂模式：
			简介：提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。
			示例：Android底层对MediaPlayer的创建。MediaPlayerFactory是Android底层为了创建不同的MediaPlayer所定义的一个类。
（3）工厂模式：
			简介：定义一个用于创建对象的接口，让子类决定将哪一个类实例化。
			示例：BitmapFactory位图工厂，专门用来将指定的图片转换为指定的位图Bitmap。
（4）原型模式：
			简介：用原型实例指定创建对象的种类，并且通过拷贝这个原型来创建新的对象。
			示例：比如我们需要一张Bitmap的几种不同格式：ARGB_8888、RGB_565、ARGB_4444、ALAPHA_8等。那我们就可以先创建一个ARGB_8888的Bitmap作为原型，在它的基础上，通过调用Bitmap.copy(Config)来创建出其它几种格式的Bitmap。另外一个例子就是Java中所有对象都有的一个名字叫clone的方法，已经原型模式的代名词了。在系统中要创建大量的对象，这些对象之间具有几乎完全相同的功能，只是在细节上有一点儿差别。
（5）建造者模式：
			简介：将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。
			示例：AlertDialog.Builder   ImageLoader的初始配置。
（6）适配器模式：
			简介：将一个类的接口转换成客户希望的另外一个接口。
			示例：不同的数据提供者使用一个适配器来向一个相同的客户提供服务。
			ListView或GridView的Adapter。
（7）桥接模式：
			简介：将抽象部分与它的实现部分分离，使它们都可以独立地变化。
			示例：Window和WindowManager之间的关系。
			在FrameWork中Window和PhoneWindow构成窗口的抽象部分，其中Window类为该抽象部分的抽象接口，PhoneWindow为抽象部分具体的实现及扩展。而WindowManager则为实现部分的基类，WindowManagerImpl则为实现部分具体的逻辑实现。
（8）装饰模式：
			简介：动态地给一个对象添加一些额外的职责。就扩展功能而言， 它比生成子类方式更为灵活。
			示例：Activity继承自ContextThemeWrapper，ContextThemeWrapper继承自ContextWrapper，ContextWrapper才是继承自Context。ContextWrapper就是我们找的装饰者。
（9）组合模式：
			简介：将对象组合成树形结构以表示“部分-整体”的层次结构。
			示例：View和ViewGroup的组合
（10）外观模式
			简介：为子系统中的一组接口提供一个一致的界面，Facade模式定义了一个高层接口，统一编程接口。
			示例：ContextImpl
（11）享元模式：
			简介：运用共享技术有效地支持大量细粒度的对象。
			示例：Message.obtainMessage通过重用Message对象来避免大量的Message对象被频繁的创建和销毁。
（12）代理模式：
			简介：为其他对象提供一个代理以控制对这个对象的访问。
			示例：所有的AIDL都一个代理模式的例子。假设一个Activity A去绑定一个Service S，那么A调用S中的每一个方法其实都是通过系统的Binder机制的中转，然后调用S中的对应方法来做到的。Binder机制就起到了代理的作用。
（13）观察者模式：
			简介：一个对象发生改变时，所有信赖于它的对象自动做相应改变。
			示例：我们可以通过BaseAdapter.registerDataSetObserver和BaseAdapter.unregisterDataSetObserver两方法来向BaseAdater注册、注销一个DataSetObserver。这个过程中，DataSetObserver就是一个观察者，它一旦发现BaseAdapter内部数据有变量，就会通过回调方法DataSetObserver.onChanged和DataSetObserver.onInvalidated来通知DataSetObserver的实现类。事件通知也是观察者模式。
（14）中介者模式：
			简介：用一个中介对象来封装一系列的对象交互。中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。
			示例：Binder机制。
(15)访问者模式:
			简介：表示一个作用于某对象结构中的各元素的操作。它使你可以在不改变各元素的类的前提下定义作用于这些元素的新操作。
			示例：编译时注解中的ElementVisitor中定义多个Visit接口，每个接口处理一种数据类型，这就是典型的访问者模式，访问者模式正好解决了数据结构和数据操作分离的问题，避免某些操作污染了数据对象类。
（16） 解释器模式：
			简介：给定一个语言，定义它的文法的一种表示，并定义一个解释器，这个解释器使用该表示来解释语言中的句子。
			示例：PackageParser这个类对AndroidManifest.xml这个配置文件的解析过程，
（17）迭代器模式
			简介：提供一种方法顺序访问一个聚合对象中各个元素, 而又不需暴露该对象的内部表示。
			示例：在Android中除了各种数据结构体，如List，Map，等包含的迭代器以外，Android源码中也提供了迭代器遍历模式，比如数据库查询使用Cursor，当我们使用SQLiteDataBase的query方法查询数据库时，会返回一个Cursor游标对象，该游标对象实际上就是一个具体的迭代器。
（18）备忘录模式
			简介：不需要了解对象的内部结构的情况下备份对象的状态，方便以后恢复。
			示例：Activity的onSaveInstanceState和onRestoreInstanceState就是通过Bundle这种序列化的数据结构来存储Activity的状态，至于其中存储的数据结构，这两个方法不用关心。
（19）责任链模式
			简介：有多个的对象可以处理一个请求，哪个对象处理该请求运行时刻自动确定。
			示例:  责任链模式在Android源码中比较类似的实现莫过于对事件的分发处理，每当用户接触屏幕时候，Android都会将对应的事件包装成一个事件对象从ViewTree的顶部至上而下的分发传递。ViewGroup事件投递的递归调用就类似一条责任链，一旦寻找到责任者，那么就由责任者持有并消费该次事件，具体的体现在View的onTouchEvent方法中的返回值，如果OnTouchEvent返回false，那么意味着当前View不会是该次事件的责任人，将不会对该事件持有。
（20）状态模式：
			简介：状态发生改变时，行为改变。
			示例：View.onVisibilityChanged方法，就是提供了一个状态模式的实现，允许在View的visibility发生改变时，引发执行onVisibilityChanged方法中的动作。
（21）策略模式
			简介：定义了一系列封装了算法、行为的对象，他们可以相互替换。
			示例：Java.util.List就是定义了一个增（add）、删（remove）、改（set）、查（indexOf）策略，至于实现这个策略的ArrayList、LinkedList等类，只是在具体实现时采用了不同的算法。但因为它们策略一样，不考虑速度的情况下，使用时完全可以互相替换使用。
（22）命令模式
			简介：把请求封装成一个对象发送出去，方便定制、排队、取消。
			示例：Handler.post后Handler.handleMessage。
（23）享元模式
			简介：运用共享技术有效地支持大量细粒度的对象。
			示例：Message.obtainMessage通过重用Message对象来避免大量的Message对象被频繁的创建和销毁。