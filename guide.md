
# Matthew's Android Cheatsheet
[Markdown Guide](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

[Youtube Guide by Bill Butterfield](https://www.youtube.com/watch?v=dFlPARW5IX8&list=PLp9HFLVct_ZvMa7IVdQyUUyh8t2re9apm)

[Online Markdown Editor](https://stackedit.io/app#)

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
* `manifests`: set things like entry point for the app
* `java`: where we find all the java code
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
**Toast**
* The mini popup message that shows up at the bottom of the screen Eg. `"loading..."`
```java
Toast toast = Toast.makeText(getApplicationContext, "Hi", Toast.LENGTH_SHORT);
toast.show();
```
* Available toast duration: `LENGTH_SHORT` (2.0s) and `LENGTH_LONG` (3.5s)
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
* Use `putExtra` to give extra information to the second activity:
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
# Creating a list of items using `ListView` + Add an image with `ImageView`

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

**Adaptor Constructor**
	* MainActivity will pass the relevant arrays needed for the item adaptor to populate all the entries
```java
public ItemAdaptor(Context context, String[] items, String[] descriptions, String[] prices) {
	this.items = items;
	this.descriptions = descriptions;
	this.prices = prices;
	m_inflator = (LayoutInflater) context.getSystemService(Context.LAYOUT_INFLATOR_SERVICE);
}
```

**GetView**
* Inflate the view, then `findViewByID` using that view (NOT using R this time)
* Once everything is set, return that view
```java
@Override
public View getview(int i, View view, ViewGroup viewGroup) {
	View v = mInflator.inflate(R.layout.my_listview_detail, null);

	TextView nameTextView = (TextView) findViewByID(v.id.nameTextView);
	TextView descriptionTextView= (TextView) findViewByID(v.id.descriptionTextView);
	TextView priceTextView= (TextView) findViewByID(v.id.priceTextView);

	String name = names[i];
	String description = descriptions[i];
	String price = prices[i];

	nameTextView.setText(name);
	descriptionTextview.setText(description);
	priceTextView.setText(price;

	return v;
}
```

**onItemClickListener**

