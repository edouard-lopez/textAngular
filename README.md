textAngular
===========

Demo is available at : http://www.textangular.com

## Requirements

1. `AngularJS` ≥ `1.2.x` ;
2. `Angular Sanitize` ≥ `1.2.x`.

### Optional requirements

1. `Bootstrap 3.0` only for the default styles ;
2. `Font-Awesome 4.0` for the default icons on the toolbar.

## Usage

1. Include `textAngular.js` in your project, alternatively grab all this code and throw it in your "`directives.js`" module file.
```html
<script src="bower_components/jquery/jquery.js"></script>
<script src="bower_components/textAngular/textAngular.js"></script>
```
2. Add a dependency to `textAngular` in your _main app module_:
```javascript
var cfdictApp = angular.module('cfdictApp',[
  // 'otherDepencendies',
  'textAngular',
]);
```
3. Create an element to hold the editor and add `ng-model="html"` attribute:
```html
<div text-angular ng-model="html"> </div>
```
or
```html
<text-angular ng-model="html" />
```
this acts in a similar fashion to the input directive of angular so if you define a name attribute you can use form validation as if it was a regular input.
4. Create a `textAngularOpts` object and bind it to your _local `$scope`_ in the controller you want controlling textAngular. It should look something like:
```javascript
$scope.textAngularOpts = {
  // options go here
}
```
5. Have fun!

I recommend to add the following `CSS` in your stylesheet to display the text box nicely:
```css
.ta-editor{
    min-height: 300px;
    height: auto;
    overflow: auto;
    font-family: inherit;
    font-size: 100%;
}
```

**Important Note:** In it's current state textAngular **does not support** the use of class or style attributes in the RAW html or in plugins.


### Setting up the Toolbar

#### Using `HTML` attributes

Several options can be set through attributes on the HTML tag, these are;

* `ta-toolbar` (*array[array, array, …]*): each element is the name of one of a tool.
<br>default:
```json
[
    ['h1', 'h2', 'h3', 'p', 'pre', 'bold', 'italics', 'ul', 'ol', 'redo', 'undo', 'clear'],
    ['html', 'insertImage', 'insertLink']
]
```
* `ta-toolbar-class` (_string_): style the **div containing the toolbar**.
    * default: _btn-toolbar_.
    * The class `ta-toolbar` (previous) is also added to the toolbar ;
* `ta-toolbar-group-class` (_string_): style the **nested groups in the toolbar**.
    * default: _btn-group_.
    * a div with this class is created for each nested array in the `ta-toolbar` array and then the tool buttons are nested inside the group,
* `ta-toolbar-button-class` (_string_): style each **tool button** in the toolbar.
    * default: _btn btn-default_.
* `ta-toolbar-active-button-class` (_string_): style each **tool button** in the toolbar if it's `activeState`'s function returns `true`, i.e. when a tool function is applied to the selected text.
    * default: _active_.
* `ta-text-editor-class` (_string_): style the **Text editor** pre tag.
    * default: _form-control_.
    * The classes `ta-editor` and `ta-text` are also added.
* `ta-html-editor-class` (_string_): style the **HTML editor** div tag.
    * default: _form-control_.
    * The classes `ta-editor` and `ta-html` are also added.

Add tools to the toolbar like:

#### Using AngularJS
```js
$rootScope.textAngularOpts = {
	toolbar: [
		['h1', 'h2', 'h3', 'p', 'pre', 'bold', 'italics', 'ul', 'ol', 'redo', 'undo', 'clear'],
		['html', 'insertImage', 'insertLink']
	],
	classes: {
		toolbar: "btn-toolbar",
		toolbarGroup: "btn-group",
		toolbarButton: "btn btn-default",
		toolbarButtonActive: "active",
		textEditor: 'form-control',
		htmlEditor: 'form-control'
	}
}
```

#### Custom buttons

The toolbar buttons are defined in the object variable `$rootScope.textAngularTools`.
The following is an example of how to add a button to make the *selected text red*:

```js
$rootScope.textAngularTools.colourRed = {
	display: "<button ng-click='action()' ng-class='displayActiveToolClass(active)'><i class='fa fa-square' style='color: red;'></i></button>",
	action: function(){
		this.$parent.wrapSelection('formatBlock', '<span style="color: red">');
	},
	activeState: function(){return false;} //This isn't required, and currently doesn't work reliably except for the html tag that doesn't rely on the cursor position.
};
//the following adds it to the toolbar to be displayed and used.
$rootScope.textAngularOpts.toolbar = [
	['colourRed', 'h1', 'h2', 'h3', 'p', 'pre', 'bold', 'italics', 'ul', 'ol', 'redo', 'undo', 'clear'],
	['html', 'insertImage', 'insertLink']
];
```

When we create a button we create an isolated child scope of the textAngular scope and extend it with the values in the tools object, we then compile the HTML in the display value with the newly created scope.

Note that the way any functions are called in the plugins the `this` variable will allways point to the scope of the button ensuring that `this.$parent` will allways
Here's the code we run for every tool:

```js
toolElement = angular.element($rootScope.textAngularTools[tool].display);
toolElement.addClass(scope.classes.toolbarButton);
groupElement.append($compile(toolElement)(angular.extend scope.$new(true), $rootScope.textAngularTools[tool]));
```

### Issues?

I've tested in `Firefox`, `Chrome` and `IE10` and its works as expected.
If you find something, please let me know.
Throw me a message, or submit a issue request!

However, `textAngular` uses `execCommand` for the rich-text functionalty.
That being said, its still a fairly **experimental browser feature-set**, and may not behave the same in all browsers.

## License
This project is licensed under the [MIT license](http://opensource.org/licenses/MIT).

## Contributers

Special thanks to all the contributions thus far!

Including those from:

* [SimeonC](https://github.com/SimeonC) ;
* [edouard-lopez](https://github.com/edouard-lopez) ;
* [108ium](https://github.com/108ium) ;
* [nadeeshacabral](https://github.com/nadeeshacabral).
