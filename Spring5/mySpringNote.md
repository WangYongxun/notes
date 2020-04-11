### 关于IOC

+ 控制反转是一种方法思路，不指定具体代码
+ **控制反转的思想**是，原本需要某个类创建（new）一个其他类的实例来应用，在需要修改时不得不修改原代码，但是为工具类创建公共接口，然后在需要使用这个类实例的类里面设置set方法并设置接口的引用，使用时由外部先设置具体实现，就不需要修改内部代码
+ 原本是类自己主动创建实例，IOC后变成由类被动接收实例，降低了耦合度
+ 使用接收对象的方式就可以解放程序代码，不用管理对象的创建，对象的来源也可以多样

#### 应用场景：

+ 某个类需要一个其他类的对象属性来执行一些操作
  
  ```java
  class A{
      private Wrench wrench = new Wrench(); // 此处由A类创建Wrench实例
      // A类需要用一个Wrench（扳手）
  }
  ```

+ 此时修改了需求，要使用其他实例来实现相同的操作就需要修改原代码
  
  ```java
  class A{
      private Hammer hammer = new Hammer();// 修改了需求就得修改原代码
      // A类现在需要一个Hammer（锤子）
  }
  ```

#### 解决思想

+ 为工具类创建一个接口，使原来的工具类都实现这个接口，在其他类需要这些对象时传入接口
  
  ```java
  class Wrench implements Tool{
      // 属于Tool接口的方法
  }
  class Hammer implements Tool{
      // 属于Tool接口的方法
  }
  ```

+ 需要这个对象时直接使用接口
  
  ```java
  class A{
      private Tool tool; // 不再由A类创建实例
  
      public void setTool (Tool tool){
          this.tool = tool; // 使用A类前由调用的地方先设置工具，不需要修改代码
      }
  
      // A类使用Tool的接口即可，不需要在意是哪个实现类
  }
  ```

#### IOC本质

+ 控制反转是设计思想，DI(依赖注入)是实现IOC的一种方法
+ 控制反转是通过描述（XML或注解）通过第三方去生产或获取特定对象的方式
+ 在Spring中实现控制反转的是IOC容器，实现方法是依赖注入（Dependency Injection）
+ Spring可以通过xml配置或者注解生成对象(bean)并实现控制反转

### 关于配置文件

#### 复杂类型的bean的配置

+ 如下面的学生类

```java
public class Student {
    private String name; // value
    private Address address; // ref
    private String[] books; // array:value
    private List<String> hobbies; // list:value
    private Map<String,String> card; // map:value
    private Set<String> games; // set:value
    private String egNull;
    private Properties info; //props:prop
}
```

+ xml里的bean配置:

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="address" class="com.kuang.pojo.Address"/>

    <bean id="student" class="com.kuang.pojo.Student">
        <!--第一种，普通值注入，value-->
        <property name="name" value="憨批"/>
        <!--第二种，Bean注入，ref-->
        <property name="address" ref="address"/>
        <!--数组注入-->
        <property name="books">
            <array>
                <value>红楼梦</value>
                <value>西游记</value>
                <value>水浒传</value>
                <value>三国演义</value>
            </array>
        </property>
        <!--List注入-->
        <property name="hobbies">
            <list>
                <value>听歌</value>
                <value>敲代码</value>
                <value>看电影</value>
            </list>
        </property>
        <!--Map-->
        <property name="card">
            <map>
                <entry key="身份证" value="1555555555"/>
                <entry key="银行卡" value="5555555555"/>
            </map>
        </property>
        <!--Set-->
        <property name="games">
            <set>
                <value>lol</value>
                <value>wow</value>
            </set>
        </property>
        <!--null-->
        <property name="egNull">
            <null/>
        </property>
        <!--Properties-->
        <property name="info">
            <props>
                <prop key="driver">com.mysql.jdbc.Driver</prop>
                <prop key="url">jdbc:mysql://localhost:3306/news</prop>
                <prop key="root">root</prop>
                <prop key="password">123456</prop>
            </props>
        </property>
    </bean>
</beans>
```

+ **加载配置文件时，Spring会把配置文件中的所有bean都创建好对应的对象并放到容器中，而且这个对象是唯一的，即使在运行时没有用到这个bean也已经在这个过程调用了构造器。**
+ 如果需要每次创建不同的bean实例，可以在配置文件的\<bean>标签里添加`scope`属性，改为原型模式
+ 多个配置文件可以由import导入到统一的一个配置文件(applicationContext)中，内容相同的bean会被合并
+ applicationContext是Spring的一个容器，它与默认配置文件名相同，即配置的bean都会被自动装载到这个容器中，同时它还有缓存，资源访问等功能，也支持其他方式注入bean。

### 注解

#### 自动装配

+ @Autowired：用在bean类的对象上，通过先类型后名字自动装配
  + 若不能唯一自动装配上属性可以使用@Qualifier(value="xxx")指定
+ @Nullable：如果字段标注了这个注解贼说明这个字段可以被装配为null
+ @Resource：用在bean类的对象上，通过先名字后类型自动装配
+ @Component：用在bean类定义的地方，说明这个类已经被Spring管理，可以被配置文件扫描到

