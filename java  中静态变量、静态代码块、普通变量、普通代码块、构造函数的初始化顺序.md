# 一：父类和子类	
```
	
	/**
	 * c
	 * 测试子类父类中静态变量、静态块、普通变量、普通快、构造函数的初始化顺序
	 *
	 * @author zsq
	 * @date 2019/1/6
	 * @time 19:35
	 */
	public class Test {
	    public static int a = getA();
	    public int aa = getAA();
	
	
	    public Test() {
	
	        System.out.println("我是父类构造函数");
	    }
	
	    static {
	        System.out.println("初始化父类中的静态代码块");
	    }
	
	    {
	        System.out.println("初始化父类中的普通代码块");
	    }
	
	    public static int getA() {
	        System.out.println("初始化父类中的静态变量");
	        return 1;
	    }
	
	    public int getAA() {
	        System.out.println("初始化父类中的普通变量");
	        return 2;
	    }
	
	}
	
	class TestSub extends Test {
	
	    public static int b = getB();
	    public int bb = getBB();
	
	    public TestSub() {
	
	        System.out.println("我是子类构造函数");
	    }
	
	    static {
	        System.out.println("初始化子类中的静态代码块");
	    }
	
	    {
	        System.out.println("初始化子类中的普通代码块");
	    }
	
	    public static int getB() {
	        System.out.println("初始化子类中的静态变量");
	        return 1;
	    }
	
	    public int getBB() {
	        System.out.println("初始化子类中的普通变量");
	        return 2;
	    }
	
	    public static void main(String[] args) {
	        TestSub  testSub=new TestSub();
	    }
	
	}

```
执行结果：

	初始化父类中的静态变量
	初始化父类中的静态代码块
	初始化子类中的静态变量
	初始化子类中的静态代码块
	初始化父类中的普通变量
	初始化父类中的普通代码块
	我是父类构造函数
	初始化子类中的普通变量
	初始化子类中的普通代码块
	我是子类构造函数


#二：接口、类、子类
```
package com.wind.selldemo;

/**
 * 接口中，不能有成员变量，只能有 public static final 修饰的变量，即使是你没有用这三个变量修饰，系统也会默认
 *
 * @author zsq
 * @date 2019/1/6
 * @time 19:53
 */

public interface Test2 {
    public static final int a = getA();
    //接口中定义普通变量，貌似没有意义
    public int aa = 1;
    //接口中不可有构造函数
//   public Test2（）{}

    //接口中不可有静态代码块
//    static  {
//        System.out.println("");
//    }
    //接口中不可有普通代码块
//    {
//
//    }


    //接口中，静态方法可以有方法体，普通方法不可以有方法体
    public static int getA() {
        System.out.println("初始化接口中的静态变量！");
        return 1;
    }

    public void getT();


    class Test implements Test2 {
        public static int b = getB();
        public int bb = getBB();


        public Test() {

            System.out.println("我是父类构造函数");
        }

        static {
            System.out.println("初始化父类中的静态代码块");
        }

        {
            System.out.println("初始化父类中的普通代码块");
        }

        public static int getB() {
            System.out.println("初始化父类中的静态变量");
            return 1;
        }

        @Override
        public void getT() {
            System.out.println();
        }

        public int getBB() {
            System.out.println("初始化父类中的普通变量");
            return 2;
        }

        //方法需调用才会执行，无论是静态还是普通
        public static void staticA() {
            System.out.println("初始化父类中的静态方法");
        }

    }

    class TestSub extends Test {

        public static int c = getC();
        public int cc = getCC();

        public TestSub() {

            System.out.println("我是子类构造函数");
        }

        static {
            System.out.println("初始化子类中的静态代码块");
        }

        {
            System.out.println("初始化子类中的普通代码块");
        }

        public static int getC() {
            System.out.println("初始化子类中的静态变量");
            return 1;
        }

        //方法需调用才会执行，无论是静态还是普通
        public static void staticC() {
            System.out.println("初始化子类中的静态方法");
        }


        public int getCC() {
            System.out.println("初始化子类中的普通变量");
            return 2;
        }

        public static void main(String[] args) {
            TestSub testSub = new TestSub();
            System.out.println("//接口中的变量，用的时候才初始化：");
            System.out.println(a);
        }

    }
}


```

执行结果：

	初始化父类中的静态变量
	初始化父类中的静态代码块
	初始化子类中的静态变量
	初始化子类中的静态代码块
	初始化父类中的普通变量
	初始化父类中的普通代码块
	我是父类构造函数
	初始化子类中的普通变量
	初始化子类中的普通代码块
	我是子类构造函数
	//接口中的变量，用的时候才初始化：
	初始化接口中的静态变量！
	1