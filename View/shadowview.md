```Java
package com.duodian.communityspace

import android.content.Context
import android.util.AttributeSet
import android.view.View
import android.graphics.*
import android.graphics.BlurMaskFilter.Blur


/**
 * Created by Lynn.
 */

class TestShadowView : View {
    constructor(context : Context) : this(context , null) {}

    constructor(context : Context , attrs : AttributeSet?) : this(context , attrs , 0) {}

    constructor(context : Context , attrs : AttributeSet? , defStyleAttr : Int) : super(context , attrs , defStyleAttr) {
    }

    constructor(context : Context , attrs : AttributeSet? , defStyleAttr : Int , defStyleRes : Int) : super(context , attrs , defStyleAttr , defStyleRes) {
    }

    init {
        paint = Paint()
        paint!!.flags = (Paint.ANTI_ALIAS_FLAG)
        paint!!.isAntiAlias = (true)
        paint!!.color = (Color.RED)
        paint!!.textSize = (20f)
        paint!!.style = (Paint.Style.FILL_AND_STROKE)
        paint!!.strokeWidth = (3f)
        paint!!.isSubpixelText = (true)

        bmf = BlurMaskFilter(28f , Blur.SOLID)
        paint!!.maskFilter = (bmf)
        rects = Rect(0 , 0 , 0 , 0)
    }

    private var paint : Paint? = null
    private var bmf : BlurMaskFilter? = null
    private var rects : Rect? = null


    override fun onDraw(canvas : Canvas?) {
        rects?.set(0 , 0 , width , 200)
        paint!!.maskFilter = bmf
        paint!!.color = /*Color.parseColor("#88000000")*/Color.RED
        canvas?.drawRect(rects , paint)
        super.onDraw(canvas)
    }
}

```
