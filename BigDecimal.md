# BigDecimal

## 1 . 概述

 Java在java.math包中提供的API类BigDecimal，用来对超过16位有效位的数进行精确的运算。双精度浮点型变量double可以处理16位有效数，但在实际应用中，可能需要对更大或者更小的数进行运算和处理。一般情况下，对于那些不需要准确计算精度的数字，我们可以直接使用Float和Double处理，但是Double.valueOf(String) 和Float.valueOf(String)会丢失精度。所以开发中，如果我们需要精确计算的结果，则必须使用BigDecimal类来操作。

 BigDecimal所创建的是对象，故我们不能使用传统的+、-、*、/等算术运算符直接对其对象进行数学运算，而必须调用其相对应的方法。方法中的参数也必须是BigDecimal的对象。构造器是类的特殊方法，专门用来创建对象，特别是带有参数的对象。

## 2. 常用构造参数

1. BigDecimal(int)

   创建一个具有参数所指定整数值的对象

2. BigDecimal(double)

   创建一个具有参数所指定双精度值的对象

3. BigDecimal(long)

   创建一个具有参数所指定长整数值的对象

4. BigDecimal(String)

   创建一个具有参数所指定以字符串表示的数值的对象



- PS :  在公司内设计公约中 ,强制要求使用 BigDecimal(String)  这个构造参数

- 因为参数类型为Double的构造方法其构造的结果是有一定不可预知性的, 而String构造方法是完全可以预知的 . 


例如:

```java
        BigDecimal a =new BigDecimal(0.1);
        System.out.println("a values is:"+a);
        System.out.println("=====================");
        BigDecimal b =new BigDecimal("0.1");
        System.out.println("b values is:"+b);
```

结果

```
a values is:0.1000000000000000055511151231257827021181583404541015625
=====================
b values is:0.1
```



## 3. 常用方法

1. add(BigDecimal)

   BigDecimal对象中的值相加，返回BigDecimal对象

2. subtract(BigDecimal)

   BigDecimal对象中的值相减，返回BigDecimal对象

3. multiply(BigDecimal)

   BigDecimal对象中的值相乘，返回BigDecimal对象

4. divide(BigDecimal)

   BigDecimal对象中的值相除，返回BigDecimal对象

5. toString()

   将BigDecimal对象中的值转换成字符串

6. doubleValue()

   将BigDecimal对象中的值转换成双精度数

7. floatValue()

   将BigDecimal对象中的值转换成单精度数

8. longValue()

   将BigDecimal对象中的值转换成长整数

9. intValue()

   将BigDecimal对象中的值转换成整数

## 4. BigDecimal大小比较

通过BigDecimal的compareTo方法 ,例如:

```
int a = bigdemical.compareTo(bigdemical2)
```

结果:

```
a = -1,表示bigdemical小于bigdemical2；
a = 0,表示bigdemical等于bigdemical2；
a = 1,表示bigdemical大于bigdemical2；
```



## 5. BigDecimal格式化

```
    NumberFormat currency = NumberFormat.getCurrencyInstance(); //建立货币格式化引用 
    NumberFormat percent = NumberFormat.getPercentInstance();  //建立百分比格式化引用 
    percent.setMaximumFractionDigits(3); //百分比小数点最多3位 
    
    BigDecimal loanAmount = new BigDecimal("15000.48"); //贷款金额
    BigDecimal interestRate = new BigDecimal("0.008"); //利率   
    BigDecimal interest = loanAmount.multiply(interestRate); //相乘
 
    System.out.println("贷款金额:\t" + currency.format(loanAmount)); 
    System.out.println("利率:\t" + percent.format(interestRate)); 
    System.out.println("利息:\t" + currency.format(interest)); 
```



结果: 

```
贷款金额: ￥15,000.48 利率: 0.8% 利息: ￥120.00
```



保留两位小数,不够自动补0

```
public class NumberFormat {
	
	public static void main(String[] s){
		System.out.println(formatToNumber(new BigDecimal("3.435")));
		System.out.println(formatToNumber(new BigDecimal(0)));
		System.out.println(formatToNumber(new BigDecimal("0.00")));
		System.out.println(formatToNumber(new BigDecimal("0.001")));
		System.out.println(formatToNumber(new BigDecimal("0.006")));
		System.out.println(formatToNumber(new BigDecimal("0.206")));
    }
	/**
	 * @desc 1.0~1之间的BigDecimal小数，格式化后失去前面的0,则前面直接加上0。
	 * 2.传入的参数等于0，则直接返回字符串"0.00"
	 * 3.大于1的小数，直接格式化返回字符串
	 * @param obj传入的小数
	 * @return
	 */
	public static String formatToNumber(BigDecimal obj) {
		DecimalFormat df = new DecimalFormat("#.00");
		if(obj.compareTo(BigDecimal.ZERO)==0) {
			return "0.00";
		}else if(obj.compareTo(BigDecimal.ZERO)>0&&obj.compareTo(new BigDecimal(1))<0){
			return "0"+df.format(obj).toString();
		}else {
			return df.format(obj).toString();
		}
	}
}
```

结果: 

```
3.44
0.00
0.00
0.00
0.01
0.21
```

