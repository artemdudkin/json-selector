# json-q
Retrieves values from JSON objects (and JavaScript objects) by css-selector-like query (includes attribute filters and array flattening).

_I am not clever enough to use XPath over objects (via [JSONPath](https://github.com/s3u/JSONPath), [ObjectPath](http://objectpath.org/) or [DefiantJs](http://defiantjs.com/)), while I like CSS selectors. [JSONSelect](https://github.com/lloyd/JSONSelect) looks promising but abadoned, [json-query](https://github.com/mmckegg/json-query) looks overcomlicated; so I chosen to save the world and create more simple query language (inspired by CSS attribute selectors)._

## Example

```js
const {get} = require('json-q');

const data = {
  a:{
    b:[
      {name:1,c:{d:1}},
      {name:2,c:{d:2}}
    ]
  }
};

get(data, "a b[.name=1] c"); //=> [{d:1}]
```

## API

### `get(object, selector)`

Returns array of all fields of _object_ from any level of nesting that satisfies _selector_.

About selectors:

- **"a"**   means: get value of all fields named "a" from all nested level of given object
- **".a"**  means: get value of field named "a" from first level of given object (i.e. object["a"])
- **"a b"** means: get all values of all fields "b", that are nested of field "a", that can be at any level of given object
- **".a.b"** means: get field "b", that is direct descendant of field "a" from first level of given object (i.e. object["a"]["b"])

And you can add filter of any depth at any level like this: **"a.b[x.y=23] c"**

You can combine filters: **"[.x=23][.y=3]"** means "items heaving field x=23 AND field y=3"

Another thing - I consider array as multiple values of field, so 

 1. arrays of arrays become flat, i.e. {a:[[1], [2,3]]} becomes {a:[1, 2, 3]}}
 
 2. you can not address array items by index, i.e.
 
```js

var data = {
  a:{
    b:{
      c:[1,2]
    }
  }
};

get(".a.b.c", data); //=> [1,2]
get(".a.b.c.0", data); //=> []


var data = {
  a:{
    b:[
      {c:1},
      {c:2}
    ]
  }
}

get(".a.b.c", data); //=> [1,2] also
```


## License

MIT
