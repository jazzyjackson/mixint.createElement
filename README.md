# ctxify
### ctxify(HTML graph, Context object) -> data interpolated into template

Clientside, presents a simple global function that converts deep objects into DOM trees

Also every child created from this tree has a props property that offers easy getting and setting of HTML attributes.

```js
ctxify({div: {
	id: 'whateverYouWant', 
	textContent: 'This becomes a text node'
}})
// <div id="whateverYouWant"> This becomes a text node </div>
```
You can provide a second argument which will serve as an object to search for keys referenced in {{curly.bracket.delimited.dot.notation}} in any string in your HTMLX graph. 

```js
let prefixStyle = fs.readFileSync('./conf/prefixConfig.css')

ctxify({"style": {
	"textContent": "{{prefixStyle}}"
}}, { prefixStyle })
```

Here, an object with a single key, prefixStyle, with the property of the contents of `.conf/prefixConfig.css`, will be transformed into a style tag with the file contents.

Serverside, provides a module for server side rendering, interpolating data objects into your HTMLX objects and returning the HTML string.

This is presently syncronous, but it doesn't have to be...

I'll admit that bin/validateGraph is a pretty amatuer attempt at keeping a stack: I thought it would be nice to give a position.subposition.subsubposition path to where the validation failed (ran into unexpected value or type), so all my functions are redefined on every recursion with the position value passed in to the new scope. There's a better way to do this or maybe a better way to use error.stack than re-implementing it, but I haven't dug into it enough.

## Curent Version: 0.2.0

Serverside interpolates context with htmlx

No weird this binding, just pass optional context as 2nd argument.

If you want to pass in text of some file to the textContent of some object, instead of having to read the file inside of the mergectx function, make sure you have the file read before you start: pass in the text as a JS object property.

## Next Version: 0.3.0

More documentation for serverside & clientside
Clientside context interpolation is coming up next

ChildNodes is an array including {tagName: {props}} objects and strings: strings should be filepaths --> I'll try to require that path, assuming its a JSON file, in which case it will be used as the sub graph for that child