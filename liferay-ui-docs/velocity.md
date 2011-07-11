Welcome to the alloy-ui wiki!

To be filled out:

## Getting Started
	* "use"-ing AlloyUI functionality
		* To ensure a component library is loaded, use AUI().use() to declare dependencies, and sandbox your code.  AUI().use() will asynchronously load any components that were not loaded by default, making it instantly available to you.  The benefit to this is that it allows your code to run as lean as possible, loading only what it needs without having to load a lot of stuff on the page first.
	* Relationship to YUI
		* Alloy UI is a UI library built on top of YUI3 javascript library.
	* JS Primer, variables, objects, and functions
	
# Working with elements
	* Finding elements
		* Finding single elements can be done by using A.one() with the following syntax:
			* var element1 = A.one('#customElementById');
			* var element2 = A.one('.custom-element-by-class');
			* var element3 = A.one('input[type=checkbox]');
			* If there are multiple elements matching the same selector, then it will return just the first element.
		* Returning an element list can be done by simply using A.all() instead.
		* So you can search for classes, ID's, or even HTML elements.  If a selector can't be found, then it will return null.
	* Traversing through elements
		* Moving through an element list is easy with Alloy, here are some methods that can be used below
			* Finding the parent of myElement with .custom-parent: myElement.ancestor('.custom-parent');
			* Finding the first child with the class name of .custom-child: myElement.one('.custom-child');
			* Finding all children with the class name of .custom-child: myElement.all('.custom-child');
	* Attributes
		* Getting attributes from a node object is as simple as using the appropriate get method, like with
			* <pre>myObject.getStyle('backgroundColor');</pre>
		* In the scenario of getting attributes from a node list object, you need to prepend the get method with an item number like so:
			* <pre>nodeListObject.item(0).getStyle('border')</pre>
	* Manipulating elements
		* Appending a new element to the nodeObject:
			* <pre>nodeObject.append('<span>New Text</span>');</pre>
		* Appending the nodeObject to another element already on the page:
			* <pre>nodeObject.appendTo('body');</pre>
		* Updating the innerHTML of an element:
			* <pre>nodeObject.html('<b>new text</b>');</pre>
		* Removing an element:
			* <pre>nodeObject.remove();</pre>
		* Creating a brand new node from scratch:
			* <pre>var newNodeObject = A.Node.create('<div id="myOtherElement">Test</div>');</pre>
	* Styling elements
		* Setting styles is as simple as using the .setStyle() method along with the appropriate css property:
			<pre>nodeObject.setStyle('backgroundColor', '#f00'); //Sets the background color to red</pre>
			<pre>nodeObject.setStyle('border', '5px solid #0c0'); //Sets a large green border</pre>
		* Setting multiple styles at once can be done with the .setStyles() method
			<pre>nodeObject.setStyles({left: 100, position: 'absolute', top: 200 });</pre>

* Events and adding behavior
	* Basic interactions
		* All Node and NodeList objects have a method called on() that can be used to specify the type of event and 
	* Advanced interactions
	* Custom events

* Ajax
	* Recieving data from the server
	* Sending data to the server
	* Datatypes - JSON, XML, and HTML
	* Polling
	* Misc. options
	
* Effects
* Plugins
* Utilities

* Components
	* Building your own component
	* Using an existing component
	* 	AutoComplete
	* Image Gallery
	* Dialog
	* Tabs
	* ...etc..
	* Misc. options