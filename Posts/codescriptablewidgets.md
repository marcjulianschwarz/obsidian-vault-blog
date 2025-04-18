---
blog-title: How to create simple iOS widgets using Scriptable and JavaScript.
blog-published: 2020-12-07
blog-tags:
  - EN
  - Scriptable
  - iOS
blog-archived: true
---


## An introduction on how to create a simple widget for iOS

You will learn how to create simple web requests, build a good-looking widget and share it with others.

Usually, an iOS widget loads some sort of data from an API, a file or the web and displays it on your home-screen. Within the Scriptable community, people agreed on a basic structure for the code.

It consists of these two methods:

+ getData()
+ createWidget(data)

In the following three steps I will show you how to implement them. 

### 1. Getting data.
A simple web request can be used to load the data. In this case, a JSON file. Notice that the getData(url) method needs the async keyword in front of it because it uses await in its body.

```
const url = "https://www.github.com/marcjulianschwarz/code/sample.json";

async function getData(url){
	let request = new Request(url);
	data = await request.loadJSON();
	return data;
}

function processData(data){
	// Process your data here
	// ...
	return processedData;
}
```

### 2. Processing data.
The processData() function can be used to further process the data to your needs. 


### 3. Build the widget.

Now it’s time to build the actual widget. Right now Scriptable has so called ListWidgets. After you created a ListWidget() object, you can add text and other media to it.
In this example the widget will display the text stored in data.sample.

```
function createWidget(data){
	var widget = new ListWidget();
	
	var text = widget.addText(data.sample);
	text.textColor = Color.dynamic(Color.black, Color.white);
	text.font = Font.regularRoundedSystemFont(13);

	widget.backgroundColor = Color.dynamic(Color.white, Color.black);

	return widget;
}
```

In the last step, all functions will be called like this:

```
let data = await getData(url);
let processedData = processData(data);

let widget = await createWidget(processedData);

Script.setWidget(widget);
Script.complete();
```

### 4. Share

Finally, you probably want to share your widget with others.
To easily achieve this, I would recommend uploading it to GitHub.
Add a good README file that explains what your widget is doing and how to install it.


### 5. Questions?
If you have any questions or need help, you can always contact me:

+ Website: [marc-julian.de](https://www.marc-julian.de/contact.html)
+ LinkedIn [@marcjulianschwarz](https://www.linkedin.com/in/marcjulianschwarz)

You can also check out the Scriptable documentation which is excellent.
There is also a nice [Scriptable discord server]() which you can visit to share your creations or get help.