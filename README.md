##### kotlin小知识（不定期更新）
###### 1. 善用kotlin扩展
    kotlin不仅可以扩展函数，还可以扩展属性，利用这一点我们可以把一些常用的工具类方法直接扩展到Context里。
    我们可以直接在kotlin文件里定义这些方法，不需要在类或者接口里去定义
```Java
  //扩展属性->屏幕高度
  val Context.screenHeight : Int
    get() = (getSystemService(Context.WINDOW_SERVICE) as WindowManager).defaultDisplay.height
  //扩展方法->显示弹窗
  private var toast : Toast? = null
  fun Context.showShort(msg : String) {
    if (toast == null) {
        initToast()
    }
    toast!!.setText(msg)
    toast!!.show()
  }
```
###### 2、kotlin的if else是有返回值的
    kotlin里是没有三目运算符的，但我们可以直接用if else来替代，当然对于一些实体判空，我们可以直接用 ?:（kotlin里的伪三目运算符,介绍如下:
    If theexpression to the left of ?: is not null, the elvis operator returns it, otherwise it returns the expression to the right.）
    也就是只有在判断空的情况下 ?: 才可以用
```Java
  fun getMax(a : Int , b : Int) : Int {
    return if (a > b) a else b
  }
  fun getLength(str : String? , defaultLength : Int) : Int {
    return str?.length ?: defaultLength
  }
```
###### 3、kotlin中的遍历
```Java
  fun getLength(list : Array<String>) {
    //闭区间
    for (i in list.indices) {
    }
    //闭区间
    for (i in 0..list.size) {
    }
    //闭区间
    for (i in list.size - 1 downTo 0) {
    }
    //左闭右开区间，每次递增2
    for (i in 0 until list.size step 2) {
    }
    //支持lambda表达式
    list.filter { it.startsWith("a") }
          .sortedBy { it }
          .map { it.toUpperCase() }
          .forEach { println(it) }
    //同上
    with(list) {
      filter { it.startsWith("a") }
      sortedBy { it }
      map { it.toUpperCase() }
      forEach { kotlin.io.println(it) }
    }
    //map遍历
    val map = mapOf("a" to 2 , "b" to 3)
    for ((a , b) in map) {
        print(a + "====" + b.toString())
    }
  }
```
###### 4、已知bug，kotlin中是支持直接在xml中写onClick事件的，但是如果在ActivityA的layou里定义了一个onClick事件methodA并在ActivityA中实现了这个方法，如果再在ActivityB中的layout里定义一个onClick事件也叫methodA但是没有实现，那么这个点击事件会调到ActivityA里
###### 5、部分特殊用法
```Java
        //with(object)
        class A {
            fun methodA() {
            }
            fun methodB() {}
        }
        val a = A()
        //apply,调用某对象的apply函数，在函数范围内，可以任意调用该对象的任意方法，并返回该对象
        //let,默认当前这个对象作为闭包的it参数，返回值是函数里面最后一行，或者指定return
        //with,是一个单独的函数并不是Kotlin中的extension，所以调用方式有点不一样，返回是最后一行，可以直接调用对象的方法，感觉像是let和apply的结合。
        //run函数和apply函数很像，只不过run函数是使用最后一行的返回，apply返回当前自己的对象。
        with(a) {
            methodA()
            methodB()
        }
```
[apply,with,let,run详细介绍](http://blog.csdn.net/guijiaoba/article/details/54615036)
```Java
        //@loop
        haha@ for (i in 1..10) {
            if (i == 3) break@haha
        }
        //infix
        infix fun Int.x(s : Int) {}
        1 x 2//相当于1.x(2) infix只能在一个参数的方法前定义
        
        //test default value
        class M() {
            fun x(s : Int , i : Int = 2 , b : Boolean = true) {
                log("$s-------$i-------$b")
            }
        }
        M().x(1 , 4)//print 1-------4-------true
        //test vararg
        fun testVararg(vararg ts : String , t : Int) {}
        testVararg("" , "" , t = 1)
```
###### 6、活学活用apply,java中的临时赋值语句如何转换为对应的kotlin语法（kotlin中不支持同时先赋值再操作的语法，但是我么可以变个方式使用）
```Java
    //Java
    public class A {
        public void sample() {
            int j = 11;
            while ((j = getNext()) > 0) {
                Log.e("AAAA", "" + j);
            }
        }
        private int x = 2;
        private int getNext() {
            Log.e("AAAA", "getNext=" + --x);
            return x;
        }
    }
    //kotlin 通过apply实现（或者通过do-while）
    class B {
        fun sample() {
            var i = 11
            while (getNext().apply { i = this } > 0) {
                Log.e("BBBB" , "$i")
            }
        }
        private var x = 2
        private fun getNext() : Int {
            Log.e("BBBB" , "getNext=" + --x)
            return x
        }
    }
```








