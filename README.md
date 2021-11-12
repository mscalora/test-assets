# Filters

#### Simple Example
<div style="margin-left: 2in">
    function bold (str) {
        return `<b>${str}</b>`;
    }
    
    template = '<%= v | bold %>',
    compiled = overtemplate(template, {filters: {bold: bold}});
    console.log(compiled({v: 'test'}));
</div>
    
## Built-in Filter Reference Index

* [escape](#escape) - encode characters as HTML entities as needed
* [trim](#trim) - remove leading and trailing whitespace
* [normalize](#normalize) - convert all sequences of whitespace to a single space or other string
* [pad](#pad) - pad a string to a character length
* [trunc](#trunc) - truncate a string to a character length
* [fit](#fit) - pad or truncate a string to a character length
* [lowerCase](#lowerCase) - convert all uppercase characters to lowercase
* [upperCase](#upperCase) - convert all lowercase characters to uppercase
* [capitalize](#capitalize) - convert the first character of a string to uppercase
* [htmlWrap](#htmlWrap) - wrap string with an HTML tag
* [colorWrap](#colorWrap) - wrap string with an HTML colored span tag
* [htmlLink](#htmlLink) - wrap a URL in a html anchor tag 
* [round](#round) - round a number to whole values or number of decimal digits
* [fixed](#fixed) - round a number to a number of decimal digits and zero pad

> **Note:** ```null``` may be substituted for optional parameters, parentheses are optional if no parameters are needed.<br>
> **Note:** Literal strings like ```"this"``` are used as the expression for the examples in the following sections. Any valid expression is equally suitable such as ```person.name``` or ```list[4].description```

### escape <a name="escape"></a>

Converts the input to a string and encode the four critical entities characters ```&```, ```<```, ```>``` and ```"``` into entities.

Template: ```"Mom & Pop" | escape```<br>
Output: ```Mom &amp; Pop```

Normally used in the non-encoding interpolation tag (```<%=```) since the escape tag already will encode HTML entities.

### trim <a name="trim"></a>

Removes leading and trailing.

Template: ```"  has space around it   " | trim```<br>
Output: ```has space around it```

Whitespace as defined by the javascript RegExp ```\s``` assertion. See: Whitespace as defined by the javascript RegExp ```\s``` assertion. See: [ECMAScript 2022 Language Specification](https://tc39.es/ecma262/#sec-white-space)

### normalize <a name="normalize"></a>

Syntax: ```normalize([sub])```

* ```sub``` - \[optional] character to use in place of whitespace sequences, default: ``` ``` (_space_)

Remove leading and trailing space and reduce any sequences of consecutive whitespace into a single space character or other string if specified.

Template: ```"   has    space         around it   " | normalize```<br>
Output: ```has space around it```

Template: ```"   has    space         around it   " | normalize("⎡GLUE⎤")```<br>
Output: ```has⎡GLUE⎤space⎡GLUE⎤around⎡GLUE⎤it```

Whitespace as defined by the javascript RegExp ```\s``` assertion. See: [ECMAScript 2022 Language Specification](https://tc39.es/ecma262/#sec-white-space)

### pad 

Syntax: ```pad( length; [char]; [side])```

* ```length``` - \[required] desired length of string
* ```char``` - \[optional] character(s) to be added to string if needed, default: ```' '```
* ```side``` - \[optional] ```start```, ```end```, ```both``` or ```center``` default: ```'end'```

Lengthen a string to the specified length by concatenating characters, by default a space, to the start, end or both sides of the input string to a specified length. `char` is a space by default and characters are added to the ```end``` (right side) of the string. If the input string already ```length``` characters long or longer, no change is made. ```both``` and ```center``` are synonymous.

Template: <code>"blue" | pad(10)</code><br>
Output: <code>blue&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code>

Template: <code>"apple" | pad(15;" -";"both")</code><br>
Output: <code> - - blue - - -</code>

### trunc <a name="trunc"></a>

Syntax: ```trunc( length; [side]; [ellipsis-char])```

* ```length``` - \[required] desired length of string
* ```side``` - \[optional] ```start```, ```end```, ```both```, ```center``` or ```ellipsis```default: ```'end'```
* ```ellipsis-char``` - \[optional] character appended to indicate truncation occurred, default: ```…```

Shorten a string to the specified length by removing characters from the start, end or both sides of the input string to a specified length. By default, characters are removed from the ```end``` (right side) of the string. A side value of ```ellipsis``` is similar to ```end``` but replaces the last character with the ellipsis character or other specified character if the string is truncated.   ```both``` and ```center``` are synonymous.

Template: <code>"abcdefghijklmnopqrstuvwxyz" | trunc(16)</code><br>
Output: <code>abcdefghijklmnop</code>

Template: <code>"whitespace is preserved" | trunc(13;"center")</code><br>
Output: <code>space is pres</code>

Template: <code>"whitespace is preserved" | trunc(13;null;"ellipsis")</code><br>
Output: <code>whitespace i…</code>

Template: <code>"whitespace is preserved" | trunc(13;null;"ellipsis","@")</code><br>
Output: <code>whitespace i@</code>

### fit <a name="fit"></a>

Make a string fit a specific length adding or removing characters as needed. ```fit``` is equivalent to using both pad and trim with the same length.

Syntax: ```fit( length; [char]; [side]; [ellipsis-char])```

* ```length``` - \[required] desired length of string
* ```char``` - \[optional] character(s) to be added to string if needed, default: ```' '```
* ```side``` - \[optional] ```start```, ```end```, ```both```, ```center``` or ```ellipsis```default: ```'end'```
* ```ellipsis-char``` - \[optional] character appended to indicate truncation occurred, default: ```…```

Template: <code>"whitespace is preserved" | fit(13;"center")</code><br>
Output: <code>space is pres</code>

Template: <code>"whitespace is preserved" | fit(32;"#";"center")</code><br>
Output: <code>#####whitespace is preserve#####</code>

Template: <code>"whitespace is preserved" | fit(13;null;"ellipsis","Ⓧ")</code><br>
Output: <code>whitespace iⓍ</code>

### lowerCase <a name="lowerCase"></a>

Convert characters in a string to lowercase according to the current locale. See: [toLocaleLowerCase on MDN](https://developer.mozilla.org/search?q=toLocaleLowerCase)

### upperCase <a name="upperCase"></a>

Convert characters in a string to uppercase according to the current locale. See: [toLocaleLowerCase on MDN](https://developer.mozilla.org/search?q=toLocaleUpperCase)

### capitalize <a name="capitalize"></a>

Convert the first character of a string to uppercase and optionally convert the other characters of a string to lowercase.

Syntax: ```capitalize``` or ```capitalize( [force])``` 

* ```force``` - \[optional] a truthy value causes lowercasing of characters after the first

Template: <code>"billieBob is COOL" | capitalize</code><br>
Output: <code>billieBob is COOL</code>

Template: <code>"billieBob is COOL" | capitalize(true)</code><br>
Output: <code>Billiebob is cool</code>

### wrapWith <a name="wrapWith"></a>

Concatenate strings to the start, end or both sides of a string optionally inserting another string (```glue```) in between.

Syntax: ```wrapWith([start]; [end]; [glue])```

* ```start``` - \[optional] string to prepend, default: _empty string_
* ```end``` - \[optional] string to append, default: _empty string_
* ```glue``` - \[optional] string to place between input and non-empty concatenated strings

Template: <code>"blue" | wrapWith("light")</code><br>
Output: <code>lightblue</code>

Template: <code>"blue" | wrapWith(null, "green")</code><br>
Output: <code>bluegreen</code>

Template: <code>"Amsterdam" | wrapWith("New", null, " ")</code><br>
Output: <code>New Amsterdam</code>

Template: <code>"person" | wrapWith("parent", "child", ".")</code><br>
Output: <code>parent.person.child</code>

Normally used in the non-encoding interpolation tag (```<%=```) since the escape tag already will encode HTML entities.

### htmlWrap <a name="htmlWrap"></a>

Place input string in a properly formatted HTML tag.

Syntax: ```htmlWrap([tag]; [classes]; [style]; [raw]; [id])```

* ```tag``` - \[optional] html tag name, default: ```div```
* ```classes``` - \[optional] contents of class attribute, default: _empty string_
* ```style``` - \[optional] contents of style attribute, default: _empty string_
* ```raw``` - \[optional] truthy value prevents HTML entity escaping, default: ```false```
* ```id``` - \[optional] contents of id attribute, default: _empty string_

>**Note:** because css styles often contain a semicolon characters and those conflict with the default parameter separator any ```%3B``` character sequences will be replaced with semicolon characters in the style parameter value.

Template: <code>"Mom & Pop" | htmlWrap</code><br>
Output: ```<div>Mom &amp; Pop</div>```

Template: <code>"Mom & Pop" | htmlWrap("p","active special","color: blue",false,"primary")</code><br>
Output: ```<p id="primary" classes="active special" style="color: blue">Mom &amp; Pop</p>```

Template: <code>"contents" | htmlWrap("p") | htmlWrap("section", false, false, true)</code>
Output: 

Template: <code>"bold and green" | htmlWrap("span",null,"font-weight: bold%3b color: green%3B")</code><br>
Output: ```<span style="font-weight: bold; color: green;">bold and green</span>```

Normally used in the non-encoding interpolation tag (```<%=```) since the escape tag already will encode HTML entities.

### colorWrap <a name="colorWrap"></a>

A convenience function similar to htmlWrap for creating an HTML tag with a color style.

Syntax: ```colorWrap([color]; [tag]; [classes]; [style]; [raw]; [id])```

* ```color``` - CSS color style property value
* _see htmlWrap for other parameter info_

Template: <code>error.message | colorWrap('p', 'red')</code><br>
Data: <code>{[{code:17,message:"Error: unexpected failure"},{code:4,message:"Error: none"}]}</code><br>
Output: ```<p style="color:red;">Error: unexpected failure</p>```

Normally used in the non-encoding interpolation tag (```<%=```) since the escape tag already will encode HTML entities.

### htmlLink <a name="htmlLink"></a>

A convenience function similar to htmlWrap for creating an HTML anchor tag, the input string is used as the URL (href) for the anchor.

Syntax: ```htmlLink([linkText]; [target]; [classes]; [style]; [raw]; [id])```

* ```linkText``` - text used as the content on the anchor tag, if ```null``` or undefined the URL is used instead
* ```target``` - string used as the target attribute of the anchor tag
* _see htmlWrap for other parameter info_

Normally used in the non-encoding interpolation tag (```<%=```) since the escape tag already will encode HTML entities.

### round <a name="round"></a>

Round input number to specified number of decimal digits.

Syntax: ```round(digits)```

* ```digits``` - number of decimal digits to the right of the decimal point

### fixed <a name="fixed"></a>

Round input number to specified number of decimal digits and format as a string with 0 padding to the right of the decimal point.

Syntax: ```fixed(digits)```

* ```digits``` - number of decimal digits to the right of the decimal point

## Custom Filters

Custom filters can be implemented with a javascript function. The first parameter passed to the filter function is the value form the expression or the output of the previous filter. The filter function is also bound (as ```this```) to an object with the following properties:

* ```value``` - the original expression value (before being transformed by any previous filters)
* ```data``` - the current data model passed to the template function with changes from any loops in scope
* ```settings``` - the current settings
* ```expression``` - the raw text of the expression excluding any filters

### Example Custom Filter

#### Loading Module
    let overtemplate = require('../src/overtemplate'), 
        template, compiled;

#### Simple Example

    function bold (str) {
        return `<b>${str}</b>`;
    }
    
    template = '<%= v | bold %>',
    compiled = overtemplate(template, {filters: {bold: bold}});
    console.log(compiled({v: 'test'}));

#### Example With Parameter

    const ROT_CHAR = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz',
        ROT_INDEXES = Array.from(ROT_CHAR).reduce((m, c, i) => {m[c] = i; return m;},{});
    
    function rotate_string (str, num) {
        let s = Array.from(str).map(c => ROT_CHAR[(ROT_INDEXES[c] + num)% ROT_CHAR.length]).join('');
        return s;
    }
    
    template = '<%= v | rot(13) %>';
    compiled = overtemplate(template, {filters: {rot: rotate_string}});
    console.log(compiled({v: 'test'}));

#### Complex Example

    const ESCAPE_ENTITIES = {
        '&': '&amp;',
        '<': '&lt;',
        '>': '&gt;',
        '"': '&quot;',
        "'": '&#x27;', // eslint-disable-line quotes
        '`': '&#x60;',
    };
    
    function escapeHTML(str) {
        let pattern = '(?:' + Object.keys(ESCAPE_ENTITIES).join('|') + ')',
            testRegExp = new RegExp(pattern),
            replaceRegExp = new RegExp(pattern, 'g');
    
        if (testRegExp.test(str)) {
            return str.replace(replaceRegExp, function(match) {
                return ESCAPE_ENTITIES[match];
            });
        }
        return str;
    }
    
    function showExpression (str, showData) {
        let html = '<table><tr><th>Type</th><th>Value</th></tr>\n';
        html += `<tr><td>Expression</td><td>${escapeHTML(this.expression)}</td></tr>\n`;
        if (showData) {
            let data = escapeHTML(JSON.stringify(this.data, null, 2));
            html += `<tr><td>Data</td><td>${data}</td></tr>\n`;
        }
        html += `<tr><td>Value</td><td>${escapeHTML(str)}</td></tr>\n`;
        html += '</table>';
        return html;
    }
    
    template = '<%= people[1].name | show %>';
    compiled = overtemplate(template, {filters: {show: showExpression}});
    console.log(compiled({people:[{name:"Bob",age:31},{name:"Sally",age:27}]}));
