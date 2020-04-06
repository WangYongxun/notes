## Lambda表达式

### 主体

+ 避免匿名内部类过多

+ 属于函数式编程的概念

+ 代码简洁，返回值确定，代码仅写出参数和函数体即可

+ 如果代码只有一行，可以省略花括号

+ 如果参数只有一个，可以小括号

+ 可以省略参数类型，要省略则全省略

  ```java
  (params)->expression[表达式];
  (params)->statement[语句];
  (params)->{statements}
  ```

### 函数式接口

+ 函数式接口(Functional Interface)：任何接口如果只包含唯一一个抽象方法，那他就是函数式接口

  ```java
  // 如Runnable
  public interface Runnable{
      public static void run();
  }
  
  Runnable r = ()->{
      // 线程体
  }
  ```

+ 函数式接口可以通过lambda表达式来创建接口对象