# Graphics Cheatsheet
# Useful Links
* [Online Markdown Editor](https://stackedit.io/app#)
* [Markdown Guide](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
* [Coursera Guide on Graphics](https://www.coursera.org/lecture/android-programming-2/graphics-and-animation-part-1-d6pOn)
---
# Topics

---
# `ImageView` and `Canvas`

* `ImageView` and `Canvas` are used
* Drawing to an `ImageView` is simpler but more restricted. Used for simple graphics with no plan to update them often
* Drawing to a `Canvas` is more complicated but more powerful and flexible. Use for complex graphics with frequent updates
* We can draw use the `Drawable` class

# Units

* [Credits](https://stackoverflow.com/questions/2025282/what-is-the-difference-between-px-dip-dp-and-sp)
* `px`: **Pixels**
	* corresponds to actual pixels on the screen.
* `dp`: **Density-Independent Pixels**
	* an abstract unit that is based on the physical density of the screen.
	* These units are relative to a 160 dpi screen, so one dp is one pixel on a 160 dpi screen.
	* The ratio of dp-to-pixel will change with the screen density, but not necessarily in direct proportion.
	* Note: The compiler accepts both "dip" and "dp", though "dp" is more consistent with "sp".
* `sp`: **Scale-Independent Pixels**
	* this is like the dp unit, but it is also scaled by the user's font size preference.
	* It is recommended you use this unit when specifying font sizes, so they will be adjusted for both the screen density and user's preference.



# Colours

* Consists of A (Alpha- Opacity), R (Red), G (Green) and B (Blue), all 4 parameters given a value in the range [0, 255]
* When colours are specified with `#` followed by 6 characters in the range [0, f], it is interpreted as RGB
* When colours are specified with `#` followed by 8 characters in the range [0, f], it is interpreted as ARGB

# `Drawable`
* Something that can be drawn eg bitmaps, colour, shape
* The following drawable classes are available:
	* `ShapeDrawable`
	* `BitmapDrawable` (A matrix of pixels)
	* `ColorDrawable` (Represents a solid colour)
* We often create a `Drawable` object and set it to a view, then let the view handle the actual drawing. Can be done either via XML file or programmatically

# Adding Image Resources

**Adding images to resources**

* Add images to `res` -> `drawables`
	* Drag and drop into the `res` folder
* Reference images using `R.drawable.image_name`
	* Image extension is not required (eg '.jpeg' is omitted)
* Image type is `int`
```java
private int getImg(int i) {
	if (i == 0) {
		return R.drawable.apple;
	}
	else {
		return R.drawable.orange;
	}
}
```

# Image Scaling
* Just copy and paste this...
* Sometimes it is safer to also check the picture height
```java
private void setPic(ImageView img, int pic) {
	Display screen = getWindowManager().getDefaultDisplay();
	BitmapFactory.Options options = new BitmapFactory.Options();
	options.inJustDecodeBounds = true;
	BitmapFactory.decodeResource(getResources(), pic, options);
	int imgWidth = options.outWidth;
	int screenWidth = screen.getWidth();
	if (imgWidth > screenWidth) {
		int ratio = Math.round((float) imgWidth / (float) screenWidth);
		options.inSampleSize = ratio;
	}
	options.inJustDecodeBounds = false;
	Bitmap scaledImg = BitmapFactory.decodeResource(getResources(), pic, options);
	img.setImageBitmap(scaledImg);
}
```
**Sample code to demonstrate images**
* In the detail activity:
```java
public class DetailActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_detail);

        Intent intent = getIntent();
        int entryID = intent.getIntExtra("entry_ID", -1);
        Toast.makeText(getApplicationContext(), "setting up picture with id " + entryID, Toast.LENGTH_SHORT).show();
        ImageView img = (ImageView) findViewById(R.id.imageView);
        if (entryID != -1) {
            setPic(img, getPic(entryID));
        }
    }

    private int getPic(int id) {
        switch (id) {
            case 0: return R.drawable.apple;
            case 1: return R.drawable.pear;
            case 2: return R.drawable.orange;
            default: return -1;
        }
    }

    private void setPic(ImageView img, int pic) {
        Display screen = getWindowManager().getDefaultDisplay();
        BitmapFactory.Options options = new BitmapFactory.Options();
        options.inJustDecodeBounds = true;
        BitmapFactory.decodeResource(getResources(), pic, options);
        int imgWidth = options.outWidth;
        int screenWidth = screen.getWidth();
        if (imgWidth > screenWidth) {
            int ratio = Math.round((float) imgWidth / (float) screenWidth);
            options.inSampleSize = ratio;
        }
        options.inJustDecodeBounds = false;
        Bitmap scaledImg = BitmapFactory.decodeResource(getResources(), pic, options);
        img.setImageBitmap(scaledImg);
    }
}
```
# Adding graphics via XML file
* There is also `layout_centreHorizontal` and `layout_centreVertical` available
```java
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/relativeLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity" >

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:contentDescription="@string/appleDesc"
        android:src="@drawable/apple" />
</RelativeLayout>
```
* Corresponding `strings.xml` file
```java
<resources>
    <string name="app_name">graphics_practice</string>
    <string name="appleDesc">This is an apple</string>
</resources>
```

# Adding Images Programmatically
* There are several rules to check out such as:
	* `LEFT_OF` (+ view ID)
	* `RIGHT_OF` (+ view ID) etc
	* Create a new `dimens.xml` resource file
	* Dimensions can be `dp`, `px` or `sp`
	* Use `android:whatever="@dimen/my_value"` to reference values 
* These applies to all views, not just `ImageView`

* Main Code
```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        RelativeLayout relativeLayout = (RelativeLayout) findViewById(R.id.relativeLayout);

        ImageView imageView = new ImageView(getApplicationContext());
        imageView.setImageDrawable(getResources().getDrawable(R.drawable.apple));

        int width = 960;
        int height = 936;

        RelativeLayout.LayoutParams parems = new RelativeLayout.LayoutParams(width, height);
        parems.addRule(RelativeLayout.CENTER_IN_PARENT);
        imageView.setLayoutParams(parems);
        relativeLayout.addView(imageView);
    }
}
```
# `dimens.xml`

* Remember to add the units!
```xml
<?xml version="1.0" encoding="utf-8"?>  
<resources>  
	<dimen name="my_value">16dp</dimen>  
</resources>
```

# ShapeDrawable
* Used for drawing primitive shapes
* Shapes are represented by subclasses of the ShapeDrawable class
	* `PathShape` for drawing lines
	* `RectShape` for drawing rectangles
	* `OvalShape` for drawing ovals and rings
* Shapes can be defined in both XML form and programmatically (See example below)

**Example: Add two overlapping ovals**

* Using the xml file
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/relativeLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity" >


    <ImageView
        android:id="@+id/imageView2"
        android:layout_width="280dp"
        android:layout_height="280dp"
        android:layout_alignParentLeft="true"
        android:layout_centerVertical="true"
        android:padding="20dp"
        app:srcCompat="@drawable/cyan_shape" />

    <ImageView
        android:id="@+id/imageView3"
        android:layout_width="280dp"
        android:layout_height="280dp"
        android:layout_alignParentRight="true"
        android:layout_centerVertical="true"
        android:padding="20dp"
        app:srcCompat="@drawable/magenta_shape" />

</RelativeLayout>
```
* `cyan_shape.xml`
```xml
 <?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="oval">

    <solid android:color="#7F00ffff" />
</shape>
```
* `magenta_shape.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="oval">

    <solid android:color="#7fff00ff" />
</shape>
```

**Example: Add two overlapping ovals programmatically**

* Just doing one oval to demonstrate how

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        int width = (int) getResources().getDimension(R.dimen.width);
        int height = (int) getResources().getDimension(R.dimen.height);
        int padding = (int) getResources().getDimension(R.dimen.padding);
        int alpha = 80;

        RelativeLayout relativeLayout = (RelativeLayout) findViewById(R.id.relativeLayout);

        // create cyan shape
        ShapeDrawable cyanShape = new ShapeDrawable(new OvalShape());
        cyanShape.getPaint().setColor(Color.CYAN);
        cyanShape.setIntrinsicWidth(width);
        cyanShape.setIntrinsicHeight(height);
        cyanShape.setAlpha(alpha);

        // put cyanShape into imageView
        ImageView cyanView = new ImageView(getApplicationContext());
        cyanView.setImageDrawable(cyanShape);
        cyanView.setPadding(padding, padding, padding, padding);

        // specify placement within the parent view
        RelativeLayout.LayoutParams cyanViewLayoutParams = new RelativeLayout.LayoutParams(
                height, width);
        cyanViewLayoutParams.addRule(RelativeLayout.CENTER_VERTICAL);
        cyanViewLayoutParams.addRule(RelativeLayout.ALIGN_PARENT_LEFT);
        cyanView.setLayoutParams(cyanViewLayoutParams);
        relativeLayout.addView(cyanView);
    }
}
```
# Text View Style Parameters

* Main xml file to demonstrate the available parameters
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/linearLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="FFCCCCCC"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView1"
        android:layout_width="match_parent"
        android:layout_height="411dp"
        android:layout_weight="1"
        android:background="@drawable/sq1"
        android:gravity="center"
        android:text="@string/text_literal"
        android:textColor="#ff00ff00"
        android:textSize="32sp"
        android:textStyle="italic"
        android:typeface="normal" />
</LinearLayout>
```
* Corresponding rectangle shape file
* `stroke` denotes the border thickness in this case
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">

    <solid android:color="@android:color/white" />

    <stroke
        android:width="3dp"
        android:color="#FF000000"
</shape>
```

# Drawing with a Canvas

* For more complex drawings
* Four things are required:
	* A bitmap- a matrix of pixels
	* A Canvas- the things that hosts the drawing calls that updates the underlying bitmap
	* A Drawing Primitive- The specific drawing operation we want to issue (Eg `Path`, `Rect`, `Text`, `Bitmap` etc)
	* A Paint Object- for setting drawing colors and style
* **`Canvas`** supports multiple drawing methods:
	* `drawText()`
	* `drawPoints()` 
	* `drawColor()`
	* `drawOval()`
	* `drawBitmap()`
* **`Paint`** sets style parameters for drawing, such as the following methods:
	* `setStrokeWidth()`- the thickness of lines
	* `setTextSize()`
	* `setColor()`
	* `setAntiAlias()`- used to smooth out an image's jagged edges


# View Animation

* Properties of views can be changed over a period of time
* Some properties include:
	* Size
	* Position
	* Transparency
	* Orientation etc
* Android provides several view animation support classes
* **`TransitionDrawable`** handles the animation between two views
* **`AnimationDrawable`** handles frame by frame animations
* **`Animation`** handles tween animations- where certain frames or moments in the animation is specified, and android interpolates to fill in the gaps
* Not to be confused with property animation- where the property of the view is edited directly.

# **`TransitionDrawable`**
* A 2-layer drawable
* The user sees the first view, then transitions to the second view
* It fades between the first and second layer

**Main**

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        TransitionDrawable transition = (TransitionDrawable) getResources().getDrawable(
                R.drawable.shape_transition);
        transition.setCrossFadeEnabled(true);

        ImageView imageView = (ImageView) findViewById(R.id.imageView);
        imageView.setImageDrawable(transition);
        
        transition.startTransition(5000);
    }
}
```

**Transition xml file**

* `android:left`, `android:right` specifies how far away from the `imageView` boundaries to draw the shape

```xml
<?xml version="1.0" encoding="utf-8"?>
<transition xmlns:android="http://schemas.android.com/apk/res/android">

    <item android:drawable="@drawable/cyan_shape"
        android:right="200dp" />
    <item android:drawable="@drawable/magenta_shape"
        android:left="200dp" />
</transition>
```

**Oval shape xml files**

* Specify the sizes also

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="oval">

    <solid android:color="#7F00ffff" />
    <size android:width="400dp" />
    <size android:height="400dp" />
</shape>
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="oval">

    <solid android:color="#7fff00ff" />
    <size android:width="400dp" />
    <size android:height="400dp" />
</shape>
```

# `AnimationDrawable`

* Animates a series of other drawables, showing each one for a period of time

**Main**
```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ImageView imageView = (ImageView) findViewById(R.id.imageView);
        imageView.setBackgroundResource(R.drawable.view_animation);
        AnimationDrawable animationDrawable = (AnimationDrawable) imageView.getBackground();
        animationDrawable.start();
    }
}
```

**View Animation XML**
* `oneshot`: true if the animation is to loop like a gif, false if not
```xml
<?xml version="1.0" encoding="utf-8"?>
<animation-list xmlns:android="http://schemas.android.com/apk/res/android"
    android:oneshot="false">

    <item
        android:drawable="@drawable/cyan_shape"
        android:duration="1000" />
    <item
        android:drawable="@drawable/magenta_shape"
        android:duration="1000" />

</animation-list>
```

# `Animation`

* A series of transformations is applied to the context of a view
* Animation timings can be manipulated to give the effect of sequential or simultaneous changes

**Main**
```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ImageView imageView = (ImageView) findViewById(R.id.imageView);
        Animation animation = AnimationUtils.loadAnimation(this, R.anim.view_animation);
        imageView.startAnimation(animation);
    }
}
```

**XML files**

* Create a new resource directory- `anim.xml`, where all the animation files are stored
* The following operations are supported:
	* alpha, rotate, scale, set and translate
* `startOffset` is the time since the start of animation that the action transformation should start

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:shareInterpolator="false">

    <alpha
        android:duration="3000"
        android:fromAlpha="0.0"
        android:interpolator="@android:anim/linear_interpolator"
        android:toAlpha="1.0" />

    <rotate
        android:duration="4000"
        android:fromDegrees="0"
        android:interpolator="@android:anim/accelerate_interpolator"
        android:pivotX="50%"
        android:pivotY="50%"
        android:startOffset="3000"
        android:toDegrees="720" />

    <translate
        android:duration="4000"
        android:fromXDelta="0"
        android:fromYDelta="0"
        android:toXDelta="100"
        android:toYDelta="100"
        android:startOffset="7000"
        android:interpolator="@android:anim/overshoot_interpolator"/>

    <scale
        android:duration="3000"
        android:fromXScale="100%"
        android:fromYScale="100%"
        android:toXScale="200%"
        android:toYScale="200%"
        android:startOffset="11000"
        android:interpolator="@android:anim/accelerate_decelerate_interpolator"/>
</set>
```

# Property Animation

* This allows us to change the properties of an object over a period of time
* View animations are restricted to the available transformations
* Sometimes we wish to do more -> Use property animations
* **`ValueAnimator`**: The timing engine; The main class that controls the animation
	* **`TimeInterpolator`**: The `ValueAnimator` contains a **`TimeInterpolator`**, which defines how values change as a function of time
* **`AnimatorUpdateListener`**: Called back at every animation frame change. This has an `onAnimationupdate` method which gets called each time a new animation frame is created.
* **`TypeEvaluator`**: Calculates a property's value at a given point in time. When we need to create our own interpolation because of the need for a custom type, this is used. This has an `evaluate` method which gets called.
* **`AnimatorSet`**: This allows us to combine individual animations to create more complex animations

**Example**
* 
