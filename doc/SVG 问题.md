    关于svg文件，切记当从其他地方copy path路径时一定要检查里面事后有科学计数法e-xxx ，否则会崩溃

    每一个path最好都不重名

    一般svg文件可能前面有m之后就不会再有L，但是在android里面m后面一定要有其他的一个符号，否则有些svg文件会显示不全 例如：
    M1.40898132 13.6809082 6.74980164 8.91381836 15.1384735 15.9726562
    当然你在预览的事后是没有问题的，但是当你在真机上运行的时候就会发现这段路径什么都没显示😒

下面可看可不看
-------------------------------------------------------------
遇到一个很奇怪的问题，在三星5.0和一些低版本的模拟器上测试app均崩溃
看log和svg有关，但是没有具体log信息
解决方法如下：[stackoverflow](http://stackoverflow.com/questions/33737192/android-vector-drawable-crash)
异常log如下：
E/AndroidRuntime(29939): FATAL EXCEPTION: main
E/AndroidRuntime(29939): Process: *.**.***, PID: 29939
E/AndroidRuntime(29939): java.lang.ArrayIndexOutOfBoundsException: length=1; index=1
E/AndroidRuntime(29939): 	at android.util.PathParser$PathDataNode.addCommand(PathParser.java:345)
E/AndroidRuntime(29939): 	at android.util.PathParser$PathDataNode.nodesToPath(PathParser.java:260)
E/AndroidRuntime(29939): 	at android.graphics.drawable.VectorDrawable$VPath.toPath(VectorDrawable.java:1265)
E/AndroidRuntime(29939): 	at android.graphics.drawable.VectorDrawable$VPathRenderer.drawPath(VectorDrawable.java:950)
E/AndroidRuntime(29939): 	at android.graphics.drawable.VectorDrawable$VPathRenderer.drawGroupTree(VectorDrawable.java:931)
E/AndroidRuntime(29939): 	at android.graphics.drawable.VectorDrawable$VPathRenderer.draw(VectorDrawable.java:938)
E/AndroidRuntime(29939): 	at android.graphics.drawable.VectorDrawable$VectorDrawableState.updateCachedBitmap(VectorDrawable.java:705)
E/AndroidRuntime(29939): 	at android.graphics.drawable.VectorDrawable.draw(VectorDrawable.java:280)
E/AndroidRuntime(29939): 	at android.view.View.getDrawableRenderNode(View.java:16503)
E/AndroidRuntime(29939): 	at android.view.View.drawBackground(View.java:16454)
E/AndroidRuntime(29939): 	at android.view.View.draw(View.java:16210)
E/AndroidRuntime(29939): 	at android.view.View.updateDisplayListIfDirty(View.java:15139)
E/AndroidRuntime(29939): 	at android.view.View.getDisplayList(View.java:15162)
E/AndroidRuntime(29939): 	at android.view.View.draw(View.java:15940)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.drawChild(ViewGroup.java:3703)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.dispatchDraw(ViewGroup.java:3497)
E/AndroidRuntime(29939): 	at android.view.View.updateDisplayListIfDirty(View.java:15134)
E/AndroidRuntime(29939): 	at android.view.View.getDisplayList(View.java:15162)
E/AndroidRuntime(29939): 	at android.view.View.draw(View.java:15940)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.drawChild(ViewGroup.java:3703)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.dispatchDraw(ViewGroup.java:3497)
E/AndroidRuntime(29939): 	at android.view.View.updateDisplayListIfDirty(View.java:15134)
E/AndroidRuntime(29939): 	at android.view.View.getDisplayList(View.java:15162)
E/AndroidRuntime(29939): 	at android.view.View.draw(View.java:15940)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.drawChild(ViewGroup.java:3703)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.dispatchDraw(ViewGroup.java:3497)
E/AndroidRuntime(29939): 	at android.view.View.draw(View.java:16222)
E/AndroidRuntime(29939): 	at android.view.View.updateDisplayListIfDirty(View.java:15139)
E/AndroidRuntime(29939): 	at android.view.View.getDisplayList(View.java:15162)
E/AndroidRuntime(29939): 	at android.view.View.draw(View.java:15940)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.drawChild(ViewGroup.java:3703)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.dispatchDraw(ViewGroup.java:3497)
E/AndroidRuntime(29939): 	at android.view.View.draw(View.java:16222)
E/AndroidRuntime(29939): 	at android.view.View.updateDisplayListIfDirty(View.java:15139)
E/AndroidRuntime(29939): 	at android.view.View.getDisplayList(View.java:15162)
E/AndroidRuntime(29939): 	at android.view.View.draw(View.java:15940)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.drawChild(ViewGroup.java:3703)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.dispatchDraw(ViewGroup.java:3497)
E/AndroidRuntime(29939): 	at android.view.View.updateDisplayListIfDirty(View.java:15134)
E/AndroidRuntime(29939): 	at android.view.View.getDisplayList(View.java:15162)
E/AndroidRuntime(29939): 	at android.view.View.draw(View.java:15940)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.drawChild(ViewGroup.java:3703)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.dispatchDraw(ViewGroup.java:3497)
E/AndroidRuntime(29939): 	at android.view.View.updateDisplayListIfDirty(View.java:15134)
E/AndroidRuntime(29939): 	at android.view.View.getDisplayList(View.java:15162)
E/AndroidRuntime(29939): 	at android.view.View.draw(View.java:15940)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.drawChild(ViewGroup.java:3703)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.dispatchDraw(ViewGroup.java:3497)
E/AndroidRuntime(29939): 	at android.view.View.updateDisplayListIfDirty(View.java:15134)
E/AndroidRuntime(29939): 	at android.view.View.getDisplayList(View.java:15162)
E/AndroidRuntime(29939): 	at android.view.View.draw(View.java:15940)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.drawChild(ViewGroup.java:3703)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.dispatchDraw(ViewGroup.java:3497)
E/AndroidRuntime(29939): 	at android.view.View.updateDisplayListIfDirty(View.java:15134)
E/AndroidRuntime(29939): 	at android.view.View.getDisplayList(View.java:15162)
E/AndroidRuntime(29939): 	at android.view.View.draw(View.java:15940)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.drawChild(ViewGroup.java:3703)
E/AndroidRuntime(29939): 	at android.view.ViewGroup.dispatchDraw(ViewGroup.java:3497)
E/AndroidRuntime(29939): 	at android.view.View.draw(View.java:16222)
E/AndroidRuntime(29939): 	at android.widget.FrameLayout.draw(FrameLayout.java:592)
E/AndroidRuntime(29939): 	at com.android.internal.policy.impl.PhoneWindow$DecorView.draw(PhoneWindow.java:2932)
E/AndroidRuntime(29939): 	at android.view.View.updateDisplayListIfDirty(View.java:15139)
E/AndroidRuntime(29939): 	at android.view.View.getDisplayList(View.java:15162)
E/AndroidRuntime(29939): 	at android.view.ThreadedRenderer.updateViewTreeDisplayList(ThreadedRenderer.java:275)
E/AndroidRuntime(29939): 	at android.view.ThreadedRenderer.updateRootDisplayList(ThreadedRenderer



