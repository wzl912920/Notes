### 不想写不想写不想写，然而我还是来写了
#### popupwindow在7.0上的问题，showAsDropDown需要官方代码有bug，故7.0版本需要单独处理，`虽然`7.1版本已经修复，`但是`popupwind的高度一定要设置成wrap_content啊，要不然这个问题仍然存在，我就遇到这个坑

####处理popupwindow点击事件问题
####在点击popupwindow时，设置了setOutsideTouchable(true)，想要在点击弹出popupwindow的按钮时如果popupwindow存在则让popupwindow dismiss，
都则再弹出。一开始想的是重写dispatachtouch和ontouch方法，后来遇到一个简单的实现方式，如下：
```Java
        popupWindow.setTouchInterceptor(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View v, MotionEvent event) {
                if (event.getAction() == MotionEvent.ACTION_OUTSIDE) {
                    popupWindow.dismiss();
                    return true;
                }
                return false;
            }
        });
```
