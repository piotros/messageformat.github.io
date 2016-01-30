---
layout: page
title: messageformat.js
---

The experience and subtlety of your program's text can be important. MessageFormat is a mechanism for handling both **pluralization** and **gender** in your applications. It can also lead to much better translations, as it's designed to support [all the languages](http://www.unicode.org/cldr/charts/latest/supplemental/language_plural_rules.html) included in the [Unicode CLDR](http://cldr.unicode.org/).

The ICU has an [official guide](http://userguide.icu-project.org/formatparse/messages) for the format. Messageformat.js supports and extends all parts of the [standard](http://icu-project.org/apiref/icu4j/com/ibm/icu/text/MessageFormat.html), with the exception of the deprecated ChoiceFormat.

There is a good slide-deck on [Plural and Gender in Translated Messages](https://docs.google.com/presentation/d/1ZyN8-0VXmod5hbHveq-M1AeQ61Ga3BmVuahZjbmbBxo/pub?start=false&loop=false&delayms=3000#slide=id.g1bc43a82_2_14) by Markus Scherer and Mark Davis. But, again, remember that many of these problems apply even if you're only outputting english.


## What problems does it solve?

Using messageformat.js, you can separate your code from your text formatting, while enabling much more humane expressions. In other words, you won't see this anymore:

> There are 1 results.  
> There are 1 result(s).  
> Number of results: 5.

These are generally unacceptable in this day and age. Not to mention the problem expands when you consider languages with 6 different pluralization rules. You may be using something like Gettext to solve this across multiple languages, but even Gettext falls flat.


## What does it look like?

With this message (shown with extra lines for simplicity):

```js
> console.log(msg)
{GENDER, select, male {He} female {She} other {They} }
found
{RESULTS, plural, =0 {no results} one {1 result} other {# results} }
in the
{CATEGORY, selectordinal, one{#st} two{#nd} few{#rd} other{#th} }
category.
```

You'll get these results:

```js
> var mfunc = new MessageFormat('en').compile(msg);

> mfunc({ GENDER: 'male', RESULTS: 1, CATEGORIES: 2 })
'He found 1 result in 2 categories.'

> mfunc({ GENDER: 'female', RESULTS: 1, CATEGORIES: 2 })
'She found 1 result in 2 categories.'

> mfunc({ GENDER: 'male', RESULTS: 2, CATEGORIES: 1 })
'He found 2 results in 1 category.'

> mfunc({ RESULTS: 2, CATEGORIES: 2 })
'They found 2 results in 2 categories.'
```


## Features

* Handles arbitrary nesting of pluralization and select rules
* Supports all ~466 languages included in the Unicode CLDR
* Works on the server and the client
* No i18n necessary &ndash; you can use it for just well-formed english sentences `UX++;`
* Speed & efficiency: Can pre-compile messages to JavaScript code
  * Great for speed: message formatting is just string concatenation
  * Run a precompiler at build time and include just  `filesize--;`
* Compatible with other languages that support MessageFormat
* Extendable with custom formatting functions
* Very whitespace tolerant
* Supports Unicode


## Install

### Node
```
npm install messageformat
```

```js
var MessageFormat = require('messageformat');
var mf = new MessageFormat('en');
```

### Browser
```
bower install messageformat
```

```html
<script src="path/to/bower_components/messageformat/messageformat.js"></script>
<script>
  var mf = new MessageFormat('en');
</script>
```


## License

You may use this software under the MIT License.

You may contribute to this software under the Dojo CLA - <http://dojofoundation.org/about/cla>


## Author

* Alex Sexton - [@SlexAxton](http://twitter.com/SlexAxton) - <http://alexsexton.com/>

## Major Contributors

* Eemeli Aro - [@eemeli](https://github.com/eemeli)


## Credits

Thanks to:

* [Bazaarvoice](https://github.com/Bazaarvoice) - my employer - for letting me do cool stuff like this.
* Google has an implementation that is similar in Google Closure, I tried to vet my code against many of their tests.
* Norbert Lindenberg for showing me how good it can be.


## Implementations in other languages

[Jeff Hansen](https://github.com/jeffijoe) ([@jeffijoe](https://twitter.com/jeffijoe)) has written an [implementation for .NET](https://github.com/jeffijoe/messageformat.net) - it's a Portable Class Library, making it possible to use on iOS, Android, Windows Phone, and pretty much any other .NET target.


## Additional tools

[icu-converter](https://github.com/alex-dow/icu-converter) is a NodeJS tool for converting message files in the [ICU Resource Bundle](http://userguide.icu-project.org/locale/resources) format into JSON or .property files.