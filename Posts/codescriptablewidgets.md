---
blog-title: How to create simple iOS widgets using Scriptable and JavaScript.
blog-published: 2020-12-07
blog-tags:
  - EN
  - Scriptable
  - iOS
blog-archived: true
---

You will learn how to create simple web requests, build a good-looking widget and share it with others.

An iOS widget often loads some sort of data from an API, a file or the web and displays it on your home-screen. Within the Scriptable community, people agreed on a basic structure for the code.

It consists of these two methods:

+ `getData()`
+ `createWidget(data)`

 
## 1. Getting data.

A simple web request downloads the necessary data. In this case, a JSON file. Notice that the `getData(url)` method needs the `async` keyword in front of it because it uses `await` in its body.

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

## 2. Processing data.

The optional `processData()` function further processes the data to your needs. 

## 3. Build the widget.

Now it’s time to build the actual widget. Right now, Scriptable has so called `ListWidget` objects. You can add text and other media to it. In this example, the widget will display the text stored in `data.sample`.

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

As a last step, call all functions in this order:

```
let data = await getData(url);
let processedData = processData(data);

let widget = await createWidget(processedData);

Script.setWidget(widget);
Script.complete();
```

## 4. Share

You probably want to share your widget with others. To achieve this, I would recommend uploading it to GitHub. Add a good `README` file that explains what your widget is doing and how to install it.

## 5. Questions?
If you have any questions or need help, you can always contact me:

+ Website: [marc-julian.com](https://www.marc-julian.coml)
+ LinkedIn [@marcjulianschwarz](https://www.linkedin.com/in/marcjulianschwarz)

You can also check out the Scriptable documentation which is great. Or join the [Scriptable discord server]() which let's you share your creations or get help.
