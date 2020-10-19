# BigDecimal

### 初始化

```
BigDecimal bigDec1 = new BigDecimal("100.13");
```
```BigDecimal bigDec1 = new BigDecimal(100.13);```  **error 会丢失精度**

### 运算
**加**       

```
//RoundingMode默认是RoundingMode.HALF_UP
MathContext mc= new MathContext(5,RoundingMode.UNNECESSARY);
BigDecimal bigDecimalSum = bigDec1.add(bigDec2,mc);
```



减**       

```
 BigDecimal subtractBigDecimal = bigDec1.subtract(bigDec2);   
```



**乘**       

```
 BigDecimal multiplyBigDecimal = bigDec1.multiply(bigDec2);
```



**除**     

```
  MathContext mc= new MathContext(6,RoundingMode.HALF_UP);
                BigDecimal divideDecimal = bigDec1.divide(bigDec2,mc);
```



**注：**
        **1** 加减乘 都可以在运算中 设置 MathContext 有效数字和舍入规则；除多了个 setScle
        2 除的方法里要设置 格式。否则除不尽直接报错
        【java.lang.ArithmeticException: Non-terminating decimal expansion; no exact representable decimal result.】
       

### 格式化的类/成员变量

**1、** **MathContext类 **

成员变量**pricision ** 有效数字
成员变量**RoundingMode枚举类**  舍入模式。 默认RoundingMode.HALF_UP 四舍五入

**用法** 

构造函数创建出来mc。加减乘除方法里传mc
    

**2、表示格式的成员变量**

BigDecimal类里的

成员变量 **scale** 小数位数,

成员变量 **RoundingMode枚举类**



 **用法：**

  1 加减乘除方法里参数传递；

  2 bigDecimal.setScale(scale,RoundingMode.xx)

​     bigDecimal.setScale(scale）里默认RoundingMode.UNNECESSARY 。不丢失精度，若丢失，抛出异常



### RoundingMode枚举类 7个模式

  **0- UP**  取上。 

 对于负数是取绝对值。eg:  -120.16->-120.2

  **1-DOWN** 取下  

  **2-CEILING** 取天花板 

​    **0和2的区别在于负数进位。**{对于负数 0-UP 是取绝对值 2-CEILING取靠近0的}。

​	eg:  -120.16->-120.1

  **3-FLOOR** 取地板

  **4-HALF_UP** 四舍五入

  **5-HALF_DOWN** 五舍六入

  **6-HALF_EVEN** **银行家舍入（四舍六入五留双）**


​    四舍六入五考虑，五后非空就进一，五后为空看奇偶，五前为偶应舍去，五前为奇要进一

​	**eg:保留二位小数：** 100.1750->100.18；100.1651,100.16509->100.17； 100.1650->100.16；

​	**解析：**

​	要舍去的第一位数是小数点后第3位。**5**。

​	若5后面有数字，非全0就进1。100.1651,100.16500001->100.17；

​	若5后面为空/全0：

 		1、左侧为奇数。进位留双 100.1750->100.18；

​     	2、左侧为偶数。不进位，舍去，留双。 100.1650->100.16；


  **7-UNNECESSARY** 保留所有的有效数字

​    

   ### 实例

```
  /**
     *@Description: 00000150分打8折的票价为多少分，多少元。输出
     */
    private static void actExample() {
        log.info("actExample start");
        BigDecimal bigDecimal = new BigDecimal("00000150");
        log.info("bigDecimal {}",bigDecimal);
        bigDecimal = bigDecimal.multiply(new BigDecimal("0.8")).setScale(0);
        log.info("bigDecimal分 {}",bigDecimal.toString());
        bigDecimal = bigDecimal.divide(new BigDecimal("100"),2,RoundingMode.UNNECESSARY);
        log.info("bigDecimal元 {}",bigDecimal);
    }
```



### BigDecimal 比较大小

```
   int i = bigDec1.compareTo(bigDec2); 1,0,-1
```





### 展示格式化 //todo

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

结果：

```
贷款金额:	￥15,000.48
利率:	0.8%
利息:	￥120.00
```







DecimalFormat df = new DecimalFormat("#.00");  // todo

df.format(bigDec).toString();