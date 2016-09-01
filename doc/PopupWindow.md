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
```Java
