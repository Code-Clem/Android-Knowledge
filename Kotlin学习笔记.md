## Kotlin学习笔记

![](http://www.kotlindoc.cn/1_0_Banner.png)

### 学习网站

#### [Kotlin从入门到放弃](https://www.jianshu.com/c/d3eac4c37b5f)
#### [Kotlin菜鸟教程](http://www.runoob.com/kotlin/kotlin-tutorial.html)

### 基本语法

#### 1、数据类型

##### 1.1、基本数据类型

| 类型 | 位宽度 |
| ----|:-----:|
| Double | 64 |
| Float | 32 |
| Long | 64 |
| Int | 32 |
| Short | 16 |
| Byte | 8 |

每一个类型都有一个toXXX方法，将该类型转成其他类型。

##### 1.2、Boolean

它有两个值，true和false，运算方式和Java一样有三种，`||`，`&&`，`!`

##### 1.3、数组

数组用Array类实现，和Java不同的地方在于，Array类有一个size属性表示数组长度，还有get和set方法，但是也可以使用array[position]的方式获取

##### 1.4、Char
Kotlin中的Char类型不能作为数字使用，如果需要的话需要使用toInt方法转换。

##### 1.5、字符串
字符串的用法和Java的差不多，这里需要注意的一点是我们可以使用字符串模版，模版表达式以美元符号开头，例如：

```
val string= "i=$i"
println(string)    //输出结果为i=10
```

#### 2、函数定义
函数定义必须使用fun关键字，参数格式为 **参数: 类型**，如果有返回值，那么在函数的最后指明，例如：

```
fun sum(a: Int, b: Int) :Int {
	return a+b
}
```

#### 3、常量与变量

##### var：
可变变量，可读并且可写

```
var <标识符> : <类型> = <初始化值>
```

##### val：
不可变常量，只能赋值一次（类似Java中的final）

##### 常量与变量都可以没有初始化值,但是在引用前必须初始化

#### 4、NULL检查机制
又名Kotlin的空安全，这是Kotlin独有的，在对于声明可空的参数，在使用时要进行判空处理，有两种处理方式，字段后面加上!!，这样可以像Java那样如果为空就报空指针异常，还有一种是字段后加?，这样可以不做处理返回null或者配合?:（相当于Java中的三元运算）做判空处理：

```
//类型后面加?表示可为空
var age: String? = "23" 
//抛出空指针异常
val ages = age!!.toInt()
//不做处理返回 null
val ages1 = age?.toInt()
//age为空返回-1
val ages2 = age?.toInt() ?: -1

```

#### 5、条件判断
if和else的使用同Java一样，但是Java中的switch替换成了when，它既可以当作表达式使用，也可以当作语句使用：

```
val x=1
when(x){
	1 -> print("x==1")    第一个条件
	2 -> print("x==2")    第二个条件
	....
	else -> {     //最终（相当于switch的default）
		print("x==xxx")
	}
}

或者还可以这样用

val y=1
when(y){
	1,2 -> print("y==1 || y==2")  //多个分支有相同的处理方式时可以用逗号隔开统一处理
	else{
		print("x==xxx")
	}
}
```

#### 6、循环
while，do whie的使用方法和Java一样，唯一变的是for循环：

```
for (i in 1..5){
	print(i)   //输出为 1 2 3 4 5
}
//用 in .. 表示在某个区间，需要注意的是这里都是闭区间

//in .. 除了能在循环中用还能在条件判断中使用，例如
if (i in 1..5){
	//如果 1<=i<=5
}
if (i !in 1..5){
	//如果 i<1 || i>5
}

for (i in 5 downTo 1){
	print(i)  //输出为 5 4 3 2 1
}
//用 downTo表示逆序输出

for (i in 1..5 step 2){
	print(i)  //输出为 1 3 5
}
//用 step表示步长，相当于Java中的i+=2

for (i in 1 until 5){
	print(i)  //输出为 1 2 3 4
}
//用 until表示开区间
```

### 范型通配符

##### out：
在java中，有通配符和边界的概念比如Class<? extends T>，表示上界通配符，它代表T以及T的子类，上限是T；在kotlin中可以使用out来替代例如`clazz: Class<out T>`

##### in：
同样也有下届通配符比如<? super T>，它表示T以及T的超类，下限是T；在kotlin中可以使用in来代替例如`clazz: Class<in T>`


### 静态类和静态方法

##### object（全局）：
在java中，我们使用static来修饰类或者方法，标示该类或方法是静态方法，在kotlin中我们可以使用object来修饰类，这样我们就可以直接 **类名.方法** 名调用静态类中的方法了，但是不能用来修饰方法。

##### companion object（部分）：
在kotlin中，我们可以使用`companion object{}`来修饰方法：

```
companion object {
    fun start(context: Context) {
        FRStartActivity.start(context, MainActivity::class.java)
    }
}
```
这样这个方法就相当于java中的静态方法，我们可以直接使用 **类名.方法** 名来进行调用。

### 构造方法
构造方法分为主构造方法和次构造方法，主构造方法只能有一个，次构造方法可以有多个：

```
class Test(context: Context, flag: Int, string: String) {

    constructor(context: Context, flag: Int) : this(context, flag, "")

    constructor(context: Context, string: String) : this(context, 0, string)
}
```

1. 主构造函数在类头中申明，而次构造函数在类体中申明；
2. 主构造函数没有任何修饰符时可以省略constructor关键字，而次构造函数不能省略；
3. 主构造函数不能包含任何的代码，而次构造函数可以；
4. 主构造函数的参数可以在类体中的属性初始化代码和初始化块中使用，而次构造函数的参数不能；
5. 主构造函数中可以直接申明并初始化属性，而次构造函数不能直接申明属性；
6. 如果申明了主构造函数，那么所有的次构造函数都必需直接或间接地委托给主构造函数；
7. 非抽象类中如果没有声明任何构造函数，会生成一个不带参数的主构造函数，而不会生成任何次构造函数。


### 常见的操作符讲解

##### 1、`:`操作符
1.1、声明常量和变量的类型

```
var count:Int=0
val TAG:String="Main"
```
1.2、类的继承

```
//注意，在Kotlin中，所有的类都是不能直接被继承的，需要添加open关键字，open关键字
//和Java中的final关键字意义相反
class MainActivity : Activity(){
}
```
1.3、使用Java类

```
val intent=Intent(this,MainActivity::class.java)
```
##### 2、`?`操作符
`?`一般在某个对象或者方法后面添加，表示该对象或者方法可以为空。

```
var name:String?=null

fun name(str:String):Int?{
}

name?.length //如果name非空，那么返回name.length，否则返回null
```

##### 3、`?:`操作符
该操作符也称之为Elvis操作符，来看一下它的使用方法：

```
val length = name?.length ?: -1 
```
如果`?:`左侧表达式非空，那么就会返回其左边表达式的结果，否则返回右边的。

##### 4、`!!`操作符

```
val length=name!!.length
```
如果name为null，那么会宝空指针异常，否则会返回name的长度，它与`?`的区别在于它不允许为空，为空就报空指针异常。

##### 5、`as`与`as?`操作符
这两个操作符都是用来类型转换的，但是前者可能会出现类型转换出错，然后会报ClassCastException异常，后者当出现类型转换的错误时会返回null。

```
private lateinit var textView : TextView
private var imageView : ImageView? = null

textView = findViewById(R.id.xxx) as TextView
imageView = findViewById(R.id.xxx) as? ImageView

```

##### 6、`is`与`!is`操作符
这两个个操作符的使用和Java中的instanceof一样，用来判断某个实例是否属于某个类型

```
if (textView is View){
}
if (imageView !is TextView){
}
```

### 扩展函数
扩展函数数是指在一个类上增加一种新的行为，甚至我们没有这个类代码的访问权限。换句话说，我们可以给某个类进行扩展，在不改变原来类的基础上增加一些新的函数方便我们使用，比如：

```
fun Context.toast(message: CharSequence, duration: Int = Toast.LENGTH_SHORT) {
    Toast.makeText(this, message, duration).show()
}

fun String?.isEmpty() = this == null || length == 0
```
对于上述函数，Context和String是函数扩展的对象，也就是你要给谁增加函数；`.`表示扩展函数修饰符；toast和isEmpty为函数名。

### 内联函数

Java的方法执行需要压栈出栈，如果一个方法被多次调用，那么就需要多次的压栈出栈，为了节省这个操作，提高一定的效率，在kotlin中使用内联函数来拷贝你调用的方法，然后在你当前方法中使用。

##### 1.1、let函数

##### 1.2、with函数

##### 1.3、run函数

##### 1.4、apply函数

##### 1.5、also函数
