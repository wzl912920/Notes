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
  }
```
###### 4、已知bug，kotlin中是支持直接在xml中写onClick事件的，但是如果在ActivityA的layou里定义了一个onClick事件methodA并在ActivityA中实现了这个方法，如果再在ActivityB中的layout里定义一个onClick事件也叫methodA但是没有实现，那么这个点击事件会调到ActivityA里
