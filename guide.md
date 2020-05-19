
  
# Matthew's Android Cheatsheet

# TODO
* Add views to layouts programmatically
---
# Useful Links
* [Markdown Guide](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
* [Youtube Guide by Bill Butterfield](https://www.youtube.com/watch?v=dFlPARW5IX8&list=PLp9HFLVct_ZvMa7IVdQyUUyh8t2re9apm)
* [Online Markdown Editor](https://stackedit.io/app#)
* [Coursera Guide on Graphics](https://www.coursera.org/lecture/android-programming-2/graphics-and-animation-part-1-d6pOn)
---
# Topics
* [Preliminaries](#preliminaries)
* [Context](#context)
* [Buttons](#buttons)
* [Intents and Second Activity](#intents-and-second-activity)
* [Creating a list of items with `ListView` ](#creating-a-list-of-items-with-listview)
* [Adding Images with `ImageView`](#adding-images-with-imageview)
* [`onTouchListener` and `MotionEvent`](#ontouchlistener-and-motionevent)
* [Dynamic Views](#dynamic-views)
* [Graphics](#graphics)
---
# Preliminaries

**Configuring a new project**
* "Choose your project" -> `Empty Activity`
* Package name -> organisation name
* Minimum API Level -> depends

**Preference- Auto Import**
* `File` -> `Settings` -> `Editor` -> `General` -> `Auto Import`
* `Ctrl + Alt + S` does `File -> Settings` for us
* `Optimize imports on the fly`
* `Add umambiguous imports on the fly`

---
**File Management**
* Found at the left panel, at the top panel (the thing with the dropdown selection)
* set the filter to `Android` to see the files that android cares about
* `manifests`: set things like entry point for the app, app permissions etc
* `java`: where we find all the java and kotlin code
* `res`: where all the resources are found
	* `drawables`: drag and drop images into it
	* `layout`: where the xml files are found
	* `values`: app-wide values are found here
		* `colors`
		* `strings`: "app_name" etc.
		* `styles`
---
**Debugging**
* Tell the program to execute the instructions one line at a time
* Add break points on the gutter (the column immediately next to the code line number to tell the code to pause once that line is reached
* `Run` -> `Debug` -> `App` (if there is only one entry point) or `MainActivity` (if there are multiple entry points)
* While the app is paused, now we can manually advance the code to the next breakpoint
* `Run` -> `Step Over`/`Step Into`/`Step Out`
* Track all objects 
---
**`MainActivity.java`**
* Entry point for the app
* On creation, it calls the corresponding xml file to set the layout of the screen
* `setContentView(R.layout.activity_main.xml)`

**`activity_main.xml`**
* Describes the layout of the screen
* Android Studios supports graphical "click and drag" style of configuration in the visual designer
* Experienced developers usually edit the xml text file directly
* Alternate between the visual and text editor by clicking `Design` and `Text` at the bottom panel
* To open an XML file, go to `app` -> `res` -> `layout` and open the file

**Display top panel**
* Click the eye icon (leftmost icon, left side of the magnet icon)
* `Show layout Decorations`

**Design and Blueprints**
* Click the double blue diamonds (leftmost fourth icon on the panel)
* `Blueprint` makes it easier to position things
* Ok to stick with `Design` view
---
**Constrained Layout**
* Allows us to position items relative to what they are tied to
* After adding a box to the screen, there are 4 available anchors that we can click and drag to tie to the available constraints on the screen
* To position an item in the middle of the screen, tie the left and right anchors to the left and right bounds of the screen
* To biased a position, click and drag the slider found below the box in the `properties` tab on the right side of the xml editor
* After tying an anchor, click and drag the box to set the offset
* It might be more convenient to switch to the text-based xml editor and enter the offset value directly
---
**RelativeLayout**
* To convet between different layouts, right click the current layout in the component tree -> `Convert View` and select layout
* Don't use this. Just use constrained layout
* To assign an id to the layout, go to the xml file and add in `android:id="@+id/myLayoutName"`	
---
**Toast**
* The mini popup message that shows up at the bottom of the screen Eg. `"loading..."`
```java
Toast toast = Toast.makeText(getApplicationContext, "Hi", Toast.LENGTH_SHORT);
toast.show();
```
* Available toast duration: `LENGTH_SHORT` (2.0s) and `LENGTH_LONG` (3.5s)
---

# Context
[MindOrks's blog](https://blog.mindorks.com/understanding-context-in-android-application-330913e32514)

* `Context` is the current state of the application
* Used to get information regarding the activity and the application
* Used to get access to databases, resources etc
* `Activity` and `Application` class extends `Context`

**Application Context**
* Analogous to a global variable
* `ApplicationContext` is tied to the life cycle of the application
* Use this if you need a context whose life cycle is not tied to the current context or when a context is being passed beyond the scope of the activity
* To initialize a library in an activity, use the `ApplicationContext` and **not** the `ActivityContext`

**Activity Context**
* Analogous to a local variable
* `ActivityContext` is tied to the life cycle of an activity
---

# Buttons

**Buttons Workflow**

* After adding a button, name the button under the `ID` section
* By convention, append the button type to the end of the name for easy identification later on
* To bind to a button:
```java
Button addButton = (Button) findViewByID(R.id.addButton);
```
**OnClickListener**
```java
addButton.setOnClickListener(new View.OnClickListener) {
	@Override
	public void onClick(View view) {
		// add actions here
	}
}
```

---
**Plain Text / Edit Text**
* `Text` -> `Plain Text` -> Click and drag into the XML screen
* Change the displayed text under the `Text` property, set to empty string if nothing is needed
* Add a faded text under the `hint` property. eg `"Enter a number"`
* Change the input value type under the `inputType` property field
	* Select `number` to read in numerical values
```java
EditText myText= (EditText) findViewByID(R.id.myText);
int num = Integer.parseInt(mytext.getText().toString());
```
**Text View**
* For displaying text only
```java
TextView myTextView = (TextView) findViewByID(R.id.myTextView);
myTextView.setText("Hello, World!");
```

**Button**
* Use this to cause actions to be performed
* `Buttons` -> `Button`
* Alternatively, `Common` -> `Button`
---
### Sample code to demonstrate button behaviours

**Addition of two numbers**
```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button addButton = (Button) findViewById(R.id.addButton);
        addButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast toast = Toast.makeText(getApplicationContext(), "Adding...", Toast.LENGTH_SHORT);
                toast.show();
                EditText firstNumEditText = (EditText) findViewById(R.id.firstNumEditText);
                EditText secondNumEditText = (EditText) findViewById(R.id.secondNumEditText);
                TextView resultTextView = (TextView) findViewById(R.id.resultTextView);
                int a = Integer.parseInt(firstNumEditText.getText().toString());
                int b = Integer.parseInt(secondNumEditText.getText().toString());
                int c = a + b;
                resultTextView.setText("" + c);
            }
        });
    }
}
```
---
# Intents and Second Activity

**Core Elements of Android Development**
* **Activity**: A rectangular box that displays something
* **Intent**: An action being requested that the device should try to perform
* **IntentService**: Services that can handle the intent requests and process the work to be done
*  **Broadcast Receivers**: Receives an Intent from a `sendBroadcast` method, often indicating that some work has been completed
---
**Creating a Second Activity**
* `app` -> `java` -> `com.blahblah` + `right click` -> `New` -> `Activity` -> `Basic Activity`
* Click `Gallery` after clicking `Activity` to see all the different types of activity layouts that are available

**Intent**
* To create an intent:
```java
Intent startIntent = new Intent(getApplicationContext(), secondActivity.class);
```
* Use `startActivity` to start the second activity:
```java
startActivity(startIntent);
```
* Use `putExtra` to give extra information to the second activity
	* It is customary to prepend the package name to the identifier name
```java
startIntent.putExtra("identifier_name", value);
```
* In the second activity, inside the `onCreate` method, use `hasExtra` to check if there was any information passed to it with the intent
```java
if (getIntent().hasExtra("identifer_name")) {
	// get the value associated with the identifier_name
	String s = getIntent().getExtras().getString("identifier_name");
}
```
**Using a web browser**
* `resolveActivity` broadcasts the request, asking android for a list of other apps that can process the activity
* If the list of apps that can perform the intent is not empty, then the activity can be started
* During the `resolveActivity` phase, the user will be prompted to pick an app from a list of apps that can perform the intent
```java
String google = "http://www.google.com";
Uri webAddr = Uri.parse(google);
Intent goToGoogle = new Intent(Intent.ACTION_VIEW, webAddr);
if (goToGoogle.resolveActivity(getPackageManager()) != null) {
	startActivity(goToGoogle);
}
```
---
**Sample code to demonstrate intents**
* **Main Activity**
```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button secondActivityButton = (Button) findViewById(R.id.secondActivityButton);
        secondActivityButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent startIntent = new Intent(getApplicationContext(), SecondActivity.class);
                startIntent.putExtra("testing", "hello there!");
                startActivity(startIntent);
            }
        });

        Button googleButton = (Button) findViewById(R.id.googleButton);
        googleButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                String google = "http://www.google.com";
                Uri webAddr = Uri.parse(google);
                Intent goToGoogle = new Intent(Intent.ACTION_VIEW, webAddr);
                if (goToGoogle.resolveActivity(getPackageManager()) != null) {
                    startActivity(goToGoogle);
                }
            }
        });
    }
}
```
* **Second Activity**
```java
public class SecondActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);

        if (getIntent().hasExtra("testing")) {
            TextView textView = (TextView) findViewById(R.id.textView);
            String s = getIntent().getExtras().getString("testing");
            textView.setText(s);
        }
    }
}
```
---
# Creating a list of items with `ListView` 

**ListView**
* `Legacy` -> `ListView`
* Populate the strings.xml file with the values desired
```xml
<resources>
    <string name="app_name">List App</string>
    
    <string-array name="items">
        <item>item 1</item>
        <item>item 2</item>
        <item>item 3</item>
    </string-array>
    
    <string-array name="prices">
        <item>$1.00</item>
        <item>$2.00</item>
        <item>$3.00</item>
    </string-array>
    
    <string-array name="descriptions">
        <item>description 1</item>
        <item>description 2</item>
        <item>description 3</item>
    </string-array>
</resources>
```
* Create a layout file for the listview
	* `App` -> `res` -> `layout` -> `new` -> `Layout Resource File`
	* Set `Root Element` = `RelativeLayout`
	* When dragging in, say, a `TextView` box, there will be arrows that displays how the box is aligned relative to the box of the entry

**List View Detail**
* Create a separate xml file that describes the layout of the entries for the list view
* This will be used by the view inflator found within the adaptor later

**Adaptors**
* An adaptor tells the `ListView` *how* to display the information in each entry
* Setup arrays containing all the required information for the adaptor to populate the list view
```java
String[] items = getResources().getStringArray(R.array.items);
String[] descriptions = getResources().getStringArray(R.array.descriptions );
```
* Create an adaptor class
	* `app` -> `java` -> select the top row -> `new` -> `Java Class`
	* Set `Superclass` = `BaseAdaptor` (`android.widget.BaseAdaptor`)
* In Android Studios, you can hover your mouse over the class name if it extends/implements another class, and click the red light bulb and click `Implement Methods`
* The following methods are required for all adaptors:
	* `getCount()`: "How many entries are there?"
	* `getItem()`: "What is the ith item?"
	* `getItemID()`: "What is the ID of this item?"
	* `getView()`: "How do I present these information?"

**Layout Inflator**
* Takes in an xml file, unwraps it, populates it with items, then wraps it up for the `getView` method
* Within the adaptor class, create a member field: `LayoutInflater`
```java
LayoutInflater mInflator; // m signifies a member field
```

**Item Adaptor Constructor**
* MainActivity will pass the relevant arrays needed for the item adaptor to populate all the entries
```java
public ItemAdaptor(Context c, String[] items, String[] descriptions, String[] prices) {
	this.items = items;
	this.descriptions = descriptions;
	this.prices = prices;
	m_inflator = (LayoutInflater) c.getSystemService(Context.LAYOUT_INFLATOR_SERVICE);
}
```

**`ItemAdaptor` -> `GetView`**
* Inflate the view, then `findViewByID` using that view (NOT using R this time)
* Once everything is set, return that view
```java
@Override
public View getview(int i, View view, ViewGroup viewGroup) {
	View v = mInflator.inflate(R.layout.my_listview_detail, null);

	TextView nameTextView = (TextView) v.findViewById(R.id.nameTextView);
	TextView descriptionTextView= (TextView) v.findViewById(R.id.descriptionTextView);
	TextView priceTextView= (TextView) v.findViewById(R.id.priceTextView);

	String name = names[i];
	String description = descriptions[i];
	String price = prices[i];

	nameTextView.setText(name);
	descriptionTextview.setText(description);
	priceTextView.setText(price;

	return v;
}
```
---
**Sample code to demonstrate `ListViews` and `ItemAdaptors`**

* Main Activity
```java
public class MainActivity extends AppCompatActivity {

    ListView myListView;
    String[] items;
    String[] descriptions;
    String[] prices;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Resources res = getResources();
        myListView = (ListView) findViewById(R.id.myListView);
        items = res.getStringArray(R.array.items);
        descriptions = res.getStringArray(R.array.descriptions);
        prices = res.getStringArray(R.array.prices);

        ItemAdaptor itemAdaptor = new ItemAdaptor(this, items, descriptions, prices);
        myListView.setAdapter(itemAdaptor);
    }
}
```
* `ItemAdaptor`
	* `my_listview_detail` is an xml file that describes the layouts of buttons within a single entry of the list view
```java
public class ItemAdaptor extends BaseAdapter {

    private LayoutInflater mInflator;
    private String[] items;
    private String[] descriptions;
    private String[] prices;

    public ItemAdaptor(Context context, String[] items, String[] descriptions, String[] prices) {
        this.items = items;
        this.descriptions = descriptions;
        this.prices = prices;
        mInflator = (LayoutInflater) context.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
    }

    @Override
    public int getCount() {
        return items.length;
    }

    @Override
    public Object getItem(int i) {
        return items[i];
    }

    @Override
    public long getItemId(int i) {
        return i;
    }

    @Override
    public View getView(int i, View view, ViewGroup viewGroup) {
        View v = mInflator.inflate(R.layout.my_listview_detail, null);

        TextView nameTextView = (TextView) v.findViewById(R.id.nameTextView);
        TextView descriptionTextView = (TextView) v.findViewById(R.id.descriptionTextView);
        TextView priceTextView = (TextView) v.findViewById(R.id.priceTextView);

        nameTextView.setText(items[i]);
        descriptionTextView.setText(descriptions[i]);
        priceTextView.setText(prices[i]);

        return v;
    }
}
```
---
**onItemClickListener**
* Tie an intent/action to a button press of something in the list view
* `int i`: i is the index of the entry that is clicked
```java
myListView.setOnItemClickListener(new AdapterView.OnItemClickListener()) {
	@Override
	public void onItemClick(AdapterView<?> adapterView, View view, int i, long l) {
		// intent
	}
}
```
---
# Adding Images with `ImageView`

**Adding images to resources**

* Add images to `res` -> `drawables`
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

**Image Scaling**
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

---
# `OnTouchListener` and `MotionEvent`

* handles the events that happen when the user touches the screen and moves the finger/mouse around
* `onClickListener` is called when the user lifts the finger
* this is called from the first user touch until the user lifts the finger
* `onTouch` returns trues to signal to the OS that this `onTouch` implementation wants to handle the touch event

**`OnTouchListener` for Buttons**
* Tell the activity to implement `View.OnTouchListener`
* Alternative methods (not recommended) is to create a separate class and pass that object in
* IDE will prompt you and say that the `OnClick` method for the `setOnTouchListener` implementation is not implemented. Ignore that.
* The X and Y values returned by the getX() and getY() method returns a value relative to the button. Manually offset these values to get the moving object to coincide with the touch location. The moving object will also be displaced relative to the layout it was fixed to (applies to both constraint and relative)
```java
public class MainActivity extends AppCompatActivity implements View.OnTouchListener{

    private ImageView imageView;
    private Button button;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        imageView = (ImageView) findViewById(R.id.imageView);
        button = (Button) findViewById(R.id.button);

        button.setOnTouchListener(this);
    }


    @Override
    public boolean onTouch(View view, MotionEvent motionEvent) {
        float x = motionEvent.getX();
        float y = motionEvent.getY();
        Toast.makeText(getApplicationContext(), x + " " + y, Toast.LENGTH_SHORT).show();
        imageView.setX(x);
        imageView.setY(y);
        return true;
    }
}
```
**`OnTouchListener` for Multiple Buttons using ID**
* Set ID for each button
* Check the ID of the view with `getID`

```java
void intialization(){
     Button m1, m2, m3, m4;
     ... //do initialization stuff
     m1.setId(1);
     m2.setId(2);
     m3.setId(3);
     m4.setId(4);
     MyTouchListener touchListener = new MyTouchListener();
     m1.setOnTouchListener(touchListener);
     m2.setOnTouchListener(touchListener);
     m3.setOnTouchListener(touchListener);
     m4.setOnTouchListener(touchListener);
 }

public class MyTouchListener implements OnTouchListener {
    @Override
    public boolean onTouch(View v, MotionEvent event) {
        switch(v.getId()){
            case 1:
                //do stuff for button 1
                break;
            case 2:
                //do stuff for button 2
                break;
            case 3:
                //do stuff for button 3
                break;
            case 4:
                //do stuff for button 4
                break;
        }
        return true;
    }

}
```

**`OnTouchListener` for Constraint Layout**
```java
public class MainActivity extends AppCompatActivity {

    private ImageView imageView;
    private ConstraintLayout myLayout = null;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        imageView = (ImageView) findViewById(R.id.imageView);
        myLayout = (ConstraintLayout) findViewById(R.id.myLayout);

        myLayout.setOnTouchListener(new ConstraintLayout.OnTouchListener() {
            @Override
            public boolean onTouch(View view, MotionEvent motionEvent) {
                float x = motionEvent.getX();
                float y = motionEvent.getY();
                imageView.setX(x);
                imageView.setY(y);
                return true;
            }
        });
    }
}
```

**`OnTouchListener` for Relative Layout**
```java
public class MainActivity extends AppCompatActivity {

    private ImageView imageView;
    private RelativeLayout myLayout = null;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        imageView = (ImageView) findViewById(R.id.imageView);
        myLayout = (RelativeLayout) findViewById(R.id.myLayout);

        myLayout.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View view, MotionEvent motionEvent) {
                float x = motionEvent.getX();
                float y = motionEvent.getY();
                imageView.setX(x);
                imageView.setY(y);
                return true;
            }
        });
    }
}
```
# Dynamic Views

* [Guide](https://exaud.com/constraintlayout/)
* Views are usually added at compile time
* Views can also be added at runtime programatically
```java
private void buildDynamicConstraintLayout() {
	// ConstraintLayout
	ConstraintLayout constraintLayout = findViewById(R.id.constraintlayout);

	// Create Button 1
	Button button1 = new Button(this);
	// Generate an Id and assign it to programmatically created Button
	button1.setId(View.generateViewId());
	button1.setText("Button 1");
	button1.setLayoutParams(new ConstraintLayout.LayoutParams(
		ViewGroup.LayoutParams.WRAP_CONTENT,
		ViewGroup.LayoutParams.WRAP_CONTENT));
	// Add programmatically created Button to ConstraintLayout
	constraintLayout.addView(button1);

	// Create Button 2
	Button button2 = new Button(this);
	// Generate an Id and assign it to programmatically created Button
	button2.setId(View.generateViewId());
	button2.setText("Button 2");
	button2.setLayoutParams(new ConstraintLayout.LayoutParams(
		ViewGroup.LayoutParams.WRAP_CONTENT,
		ViewGroup.LayoutParams.WRAP_CONTENT));
	// Add programmatically created Button to ConstraintLayout
	constraintLayout.addView(button2);

	// Create ConstraintSet
	ConstraintSet constraintSet = new ConstraintSet();
	// Make sure all previous Constraints from ConstraintLayout are not lost
	constraintSet.clone(constraintLayout);

	// Create Rule that states that the START of Button 1 will be positioned at the END of Button 2
	constraintSet.connect(button2.getId(), ConstraintSet.START, button1.getId(), ConstraintSet.END);
	constraintSet.applyTo(constraintLayout);
}
```
# Graphics

**2D Graphics Overview**
* `ImageView` and `Canvas` are used
* Drawing to an `ImageView` is simpler but more restricted. Used for simple graphics with no plan to update them often
* Drawing to a `Canvas` is more complicated but more powerful and flexible. Use for complex graphics with frequent updates
* We can draw use the `Drawable` class

**Units**
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


**Drawable**
* Something that can be drawn eg bitmaps, colour, shape
* The following drawable classes are available:
	* `ShapeDrawable`
	* `BitmapDrawable` (A matrix of pixels)
	* `ColorDrawable` (Represents a solid colour)
* We often create a `Drawable` object and set it to a view, then let the view handle the actual drawing. Can be done either via XML file or programmatically

**Adding graphics via XML file**
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

**Adding image programmatically**
* There are several rules to check out such as:
	* `LEFT_OF` (+ view ID)
	* `RIGHT_OF` (+ view ID) etc
	* Create a new `dimens.xml` resource file
	* Dimensions can be `dp`, `px` or `sp`
	* Use `android:whatever="@dimen/my_value"` to reference values 
```xml
<?xml version="1.0" encoding="utf-8"?>  
<resources>  
	<dimen name="my_value">16dp</dimen>  
</resources>
```
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

**ShapeDrawable**
* Used for drawing primitive shapes
* Shapes are represented by subclasses of the ShapeDrawable class
	* `PathShape` for drawing lines
	* `RectShape` for drawing rectangles
	* `OvalShape` for drawing ovals and rings
* Shapes can be defined in both XML form and programmatically (See example below)

**Example: Drawing two ovals, both of different colours, and both overlap**
