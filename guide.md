## Matthew's Android Cheatsheet
[Markdown Guide](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

[Youtube Guide by Bill Butterfield](https://www.youtube.com/watch?v=dFlPARW5IX8&list=PLp9HFLVct_ZvMa7IVdQyUUyh8t2re9apm)

[Online Markdown Editor](https://stackedit.io/app#)

---
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
	* `drawables`: pictures
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
### Sample Codes

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
