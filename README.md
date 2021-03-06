# sparkartSuggest

jQuery plugin for suggesting results in text inputs.

## Usage

In order to use sparkartSuggest you'll need to include jQuery, jquery.sparkartSuggest.js, and jquery.sparkartSuggest.css.

**A basic suggest input**

```html
<input id="suggestive" type="text" value="javascript,jquery" />
<script>
	$('#suggestive').sparkartSuggest({
		source: ['java','javascript','java beans','java the country','java strikes back']
	});
</script>
```

## Configuration

* **asSource** - *(array of strings)* - An array of strings to use as suggestions.
* **fnSource** - *(function)* - Alternatively, pass a function that returns a list of suggestions. An example source function:

```javascript
function( string, options, callback ){
	$.getJSON( '/suggestions?q='+ string, function( suggestions ){
		callback( suggestions );
	});
}
```

* **iThreshold** - *(integer)* - The number of characters that must be typed before suggestions start to appear. Defaults to `2`.
* **iMax** - *(integer)* - The maximum number of suggestions to show. Defaults to `8`.
* **iDelay** - *(integer)* - How long (in milliseconds) the plugin waits before loading suggestions. Defaults to `150`.
* **bDisableDefaultAutocomplete** - *(boolean)* - Many browsers have a built-in autocomplete option that can interfere with this plugin, this will attempt to disable that.  Defaults to `true`.
* **aiDisabledKeycodes** - *(array of integers)* - Array of keyCode integers representing keys we do not want events fired on.

```javascript
var DEFAULT_DISABLED_KEYCODES = [16, 17, 18, 19, 20, 33, 34, 35, 36, 37, 39, 45, 91, 92, 93,
	112, 113, 114, 115, 116, 117, 118, 119, 120, 121, 123, 124, 125, 144, 145];
```

* **fnSorter** - *(function)* - A function used to sort returned results. This is the default sorter:

```javascript
function( a, b ){
	return ( a < b )? -1: ( a > b )? 1: 0;
}
```

* **fnComparator** - *(function)* - A function used to determine which strings match. This is the default comparator:

```javascript
function( source, string ){
	// http://stackoverflow.com/questions/3446170/escape-string-for-use-in-javascript-regex#answer-6969486
	var regex_safe_string = string.replace( /[\-\[\]\/\{\}\(\)\*\+\?\.\\\^\$\|]/g, '\\$&' );
	var regex = new RegExp( '^'+ regex_safe_string, 'i' );
	return regex.test( source ) && string !== source;
}
```

## Language

SparkartSuggest now has very basic Language abilities that allow the end user to define text to be displayed during various processes.

* **bUseLanguage** - *(boolean)* - Whether to use the Language features of Sparkart Suggest.  Defaults to `false`.

* **oLanguage** - *(hash)* - Hash of string to use for various language outputs.  Default object:

```javascript
var DEFAULT_LANGUAGE = {
    "sSuggestionInfo" : "__COUNT__ found",
    "sNoSuggestions" : "No suggestions found",
    "sLoadingSuggestions" : "Loading suggestions..."
};
```
### Language Special Words

SparkartSuggest's language implementation supports special words in the following syntax: "__[A-Z]{1,}__".  You may use any uppercase alpha character in between the set of double-underscores.

By default:

```javascript
var DEFAULT_LANGUAGE_FN = {
    "__COUNT__" : function(suggestions){
        return ($(suggestions).length > 1) ? $(suggestions).length + " suggestions" : "1 suggestion";
    }
};
```

As you can see, by default only __COUNT__ is supported, but you may add you own via:

* **oLanguageFn** - *(hash)* - Hash of functions to use in Language parsing.  Exmaple:

```javascript
"oLanguageFn" : {
	"__SANDWICH__" : function() {
		var d = new Date();
		var weekday=new Array(7);
		weekday[0]="Sunday";
		weekday[1]="Monday";
		weekday[2]="Tuesday";
		weekday[3]="Wednesday";
		weekday[4]="Thursday";
		weekday[5]="Friday";
		weekday[6]="Saturday";
		return weekday[d.getDay()] + " sandwich";
	},
	"__COUNT__" : function(suggestions){
        return ($(suggestions).length > 1) ? $(suggestions).length + " suggestions" : "1 suggestion";
    }
}
```

In conjunction with:

```javascript
"oLanguage" : {
	"sSuggestionInfo" : "My this is a great day for a tasty __SANDWICH__!  Also, I found __COUNT__!",
	"sNoSuggestions" : "No suggestions found",
    "sLoadingSuggestions" : "Loading suggestions..."
}
```

The possibilities are....pretty limited right now but that will change!

## Before & After function hooks

Available hooks:

* fnBeforeSource      
* fnAfterSource       
* fnBeforeActive      
* fnAfterActive       
* fnBeforeInactive    
* fnAfterInactive     
* fnBeforeUpdate      
* fnAfterUpdate       
* fnBeforeSelect      
* fnAfterSelect       
* fnBeforeSuggestions 
* fnAfterSuggestions  
* fnBeforeHighlight   
* fnAfterHighlight    
* fnBeforeNext        
* fnAfterNext         
* fnBeforePrevious    
* fnAfterPrevious     
* fnBeforeDestroy     
* fnAfterDestroy      
* fnBeforeInit        
* fnAfterInit         

## License

MIT License.

----------

Copyright © 2012 Sparkart Group, Inc.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
