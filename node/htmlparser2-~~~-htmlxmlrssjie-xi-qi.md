#### htmlparser2是一个快速和宽容的HTML/XML/RSS解析器，解析器可以出来流，并且提供了一个回调接口。

```js

var htmlparser = require("htmlparser2");
var parser = new htmlparser.Parser({
    onopentag: function(name, attribs){
        if(name === "script" && attribs.type === "text/javascript"){
            console.log("JS! Hooray!");
        }
    },
    ontext: function(text){
        console.log("-->", text);
    },
    onclosetag: function(tagname){
        if(tagname === "script"){
            console.log("That's it?!");
        }
    }
}, {decodeEntities: true});
parser.write("Xyz <script type='text/javascript'>var foo = '<<bar>>';</ script>");
parser.end();
```