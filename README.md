# javaDoubleCalcProblem
针对java中浮点型数据（double为例）计算后精度显示异常的处理

一、具体问题：
  对两个double类型的值进行运算，有时会出现结果值异常的问题。比如： 
     System.out.println(19.99+20);
     System.out.println(1.0-0.66);
     System.out.println(0.033*100);
     System.out.println(12.3/100);
输出：
39.989999999999995
0.33999999999999997
3.3000000000000003
0.12300000000000001

二、问题分析与解决：
　　Java中的简单浮点数类型float和double不能够进行运算，因为大多数情况下是正常的，但是偶尔会出现如上所示的问题。这个问题其实不是JAVA的bug，因为计算机本身是二进制的，而浮点数实际上只是个近似值，所以从二进制转化为十进制浮点数时，精度容易丢失，导致精度下降。
　　要保证精度就要使用BigDecimal类，而且不能直接从double直接转BigDecimal，要将double转string再转BigDecimal。也就是不能使用BigDecimal(double val) 方法，你会发现没有效果。要使用BigDecimal(String val) 方法。具体例子如下所示。
 
double类型四则运算例子：
1.相加
   public static double add(double a1, double b1) {  
         BigDecimal a2 = new BigDecimal(Double.toString(a1));  
         BigDecimal b2 = new BigDecimal(Double.toString(b1));  
         return a2.add(b2).doubleValue();  
     }
 
2.相减
     public static double sub(double a1, double b1) {  
         BigDecimal a2 = new BigDecimal(Double.toString(a1));  
         BigDecimal b2 = new BigDecimal(Double.toString(b1));  
         return a2.subtract(b2).doubleValue();  
     }
 
3.相乘
     public static double mul(double a1, double b1) {  
         BigDecimal a2 = new BigDecimal(Double.toString(a1));  
         BigDecimal b2 = new BigDecimal(Double.toString(b1));  
         return a2.multiply(b2).doubleValue();  
     }
 
4.相除
     public static double div(double a1, double b1, int scale) {
         if (scale < 0) {  
             throw new IllegalArgumentException("error");  
         }
         BigDecimal a2 = new BigDecimal(Double.toString(a1));  
         BigDecimal b2 = new BigDecimal(Double.toString(b1));  
         return a2.divide(b2, scale, BigDecimal.ROUND_HALF_UP).doubleValue();  
     }
scale参数为除不尽时，指定精度。
