
```Java
package com.pj.register.tool;

import android.content.Context;
import android.graphics.PointF;
import android.support.v4.view.ViewPager;
import android.util.AttributeSet;
import android.view.MotionEvent;

/**
 * Created by wzl on 2016/5/9.
 */
public class InnerViewPager extends ViewPager {
    public InnerViewPager(Context context) {
        super(context);
    }

    public InnerViewPager(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    private PointF downPoint = new PointF();

    @Override
    public boolean onTouchEvent(MotionEvent ev) {
        switch (ev.getAction()) {
            case MotionEvent.ACTION_DOWN:
                downPoint.x = ev.getX();
                downPoint.y = ev.getY();
                if (this.getChildCount() > 1) {
                    getParent().requestDisallowInterceptTouchEvent(true);
                }
                break;
            case MotionEvent.ACTION_MOVE:
                if (this.getChildCount() > 1 && (ev.getX() < downPoint.x && getCurrentItem() + 1 < getChildCount()) || (ev.getX() > downPoint.x && getCurrentItem() != 0)) {
                    getParent().requestDisallowInterceptTouchEvent(true);
                }
                break;
            case MotionEvent.ACTION_UP:
//                if (PointF.length(ev.getX() - downPoint.x, ev.getY() - downPoint.y) < (float) 5.0) {
//                    //根据需要自行实现
//                    return true;
//                }
                break;
            default:
                break;
        }
        return super.onTouchEvent(ev);
    }
}

```Java
