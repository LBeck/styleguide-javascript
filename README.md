# HowAboutWe JavaScript Style Guide

*A mostly reasonable approach to JavaScript*


## <a name='TOC'>Table of Contents</a>

  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Conditional Expressions & Equality](#conditionals)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Leading Commas](#leading-commas)
  1. [Semicolons](#semicolons)
  1. [Type Coercion](#type-coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Constructors](#constructors)
  1. [jQuery](#jquery)
  1. [Modules](#modules)
  1. [ES5 Compatability](#es5)
  1. [Performance](#performance)
  1. [Resources](#resources)
  1. [The JavaScript Style Guide Guide](#guide-guide)
  1. [Contributors](#contributors)
  1. [License](#license)

## <a name='objects'>Objects</a>

  - Use the literal syntax for object creation.

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```

  - Don't use [reserved words](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Reserved_Words) as keys.

    ```javascript
    // bad
    var superman = {
      class: 'superhero',
      default: { clark: kent },
      private: true
    };

    // good
    var superman = {
      klass: 'superhero',
      defaults: { clark: kent },
      hidden: true
    };
    ```
    **[[⬆]](#TOC)**

## <a name='arrays'>Arrays</a>

  - Use the literal syntax for array creation

    ```javascript
    // bad
    var items = new Array();

    // good
    var items = [];
    ```

  - For clarity and [performance reasons](http://jsperf.com/caching-array-length/4), cache the length of your arrays in variables outside of the loop

    ```javascript
    // bad
    for (var i = 0; i < items.length; i++) {
      // ...stuff...
    }

    // bad
    for (var i = 0, len = items.length; i < len; i++) {
      // ...stuff...
    }

    // good
    var len = items.length;
    var i;

    for (i = 0; i < len; i++) {
      // ...stuff...
    }
    ```

  - For [performance reasons](http://jsperf.com/array-direct-assignment-vs-push/5) use direct assignment over Array#push

    ```javascript
    var len = items.length;
    var itemsCopy = [];
    var i;

    // bad
    for (i = 0; i < len; i++) {
      itemsCopy.push(items[i])
    }

    // good
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }
    ```
    **[[⬆]](#TOC)**


## <a name='strings'>Strings</a>

  - Use single quotes `''` for strings

    ```javascript
    // bad
    var name = "Bob Parr";

    // good
    var name = 'Bob Parr';

    // bad
    var fullName = "Bob" + this.lastName;

    // good
    var fullName = 'Bob' + this.lastName;

    // bad
    var fullName = "<a href=\"/name\">Bob " + this.lastName + "</a>";

    // good
    var fullName = '<a href="/name">Bob ' + this.lastName + '</a>';
    ```

  - Use double quotes `""` for interpreted strings inside templates.

    ```javascript
    // bad
    var name = users['#{ getUser }'];

    // good
    var name = users["#{ getUser }"];
    ```

  - Strings longer than 80 characters should be written across multiple lines using string concatenation.

    ```javascript
    // bad
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    var errorMessage = 'This is a super long error that \
    was thrown because of Batman. \
    When you stop to think about \
    how Batman had anything to do \
    with this, you would get nowhere \
    fast.';


    // good
    var errorMessage = 'This is a super long error that ' +
      'was thrown because of Batman.' +
      'When you stop to think about ' +
      'how Batman had anything to do ' +
      'with this, you would get nowhere ' +
      'fast.';
    ```

  - When programatically building up a string, use Array#join instead of string concatenation. Mostly for IE: [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items;
    var messages;
    var length;
    var i;

    messages = [{
        state: 'success',
        message: 'This one worked.'
    },{
        state: 'success',
        message: 'This one worked as well.'
    },{
        state: 'error',
        message: 'This one did not work.'
    }];

    length = messages.length;

    // bad
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // good
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        items[i] = '<li>' + messages[i].message + '</li>';
      }

      return '<ul>' + items.join('') + '</ul>';
    }
    ```

    **[[⬆]](#TOC)**


## <a name='functions'>Functions</a>

  - Function expressions:

    ```javascript
    // anonymous function expression
    var anonymous = function() {
      return true;
    };

    // named function expression
    var named = function named() {
      return true;
    };

    // immediately-invoked function expression (IIFE)
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    if (currentUser) {
      var test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - Never name a parameter `arguments`, this will take precendence over the `arguments` object that is given to every function scope.

    ```javascript
    // bad
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // good
    function yup(name, options, args) {
      // ...stuff...
    }
    ```

    **[[⬆]](#TOC)**



## <a name='properties'>Properties</a>

  - Use dot notation when accessing properties.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // bad
    var isJedi = luke['jedi'];

    // good
    var isJedi = luke.jedi;
    ```

  - Use subscript notation `[]` when accessing properties with a variable.

    ```javascript
    var users = {
      jane: {},
      john: {}
    };

    var userName = getUsername();
    var userObject = users[userName];
    ```

    **[[⬆]](#TOC)**


## <a name='variables'>Variables</a>

  - Always use `var` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    var superPower = new SuperPower();
    ```

  - Use additional `var` declarations for multiple variables and declare each variable on a newline. This is useful when reordering variables and avoiding simple syntax mistakes.

    ```javascript
    // bad
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    var items = getItems()
        , goSportsTeam = true
        , dragonball = 'z';

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

  - Assign variables at the top of their scope. This helps avoid issues with variable declaration and assignment hoisting related issues.

    ```javascript
    // bad
    function() {
      test();
      console.log('doing stuff..');

      //..other stuff..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // good
    function() {
      var name = getName();

      test();
      console.log('doing stuff..');

      //..other stuff..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // bad
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // good
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();

      return true;
    }
    ```

    **[[⬆]](#TOC)**


## <a name='conditionals'>Conditional Expressions & Equality</a>

  - Use `===` and `!==` over `==` and `!=`.
  - Conditional expressions are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:

    + **Objects** evaluate to `**true**`
    + **Undefined** evaluates to `**false**`
    + **Null** evaluates to `**false**`
    + **Booleans** evaluate to **the value of the boolean**
    + **Numbers** evalute to `**false**` if **+0, -0, or NaN**, otherwise `**true**`
    + **Strings** evaluate to `**false` if an **empty string `''`**, otherwise `**true**`

    ```javascript
    if ([0]) {
      // true
      // An array is an object, objects evaluate to true
    }
    ```

  - Use shortcuts.

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }
    ```

  - For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll

    **[[⬆]](#TOC)**


## <a name='blocks'>Blocks</a>

  - Use braces with all blocks.

    ```javascript
    // bad
    if (test)
      return false;

    // bad
    if (test) return false;

    // good
    if (test) {
      return false;
    }
    ```

  - Always put blocks and curlys on their own lines.

    ```javascript
    // bad
    if (test) { return false; }

    // good
    if (test) {
      return false;
    }

    // bad
    function() { return false; }

    // bad
    function() {
        return false; }

    // good
    function() {
      return false;
    }
    ```

    **[[⬆]](#TOC)**


## <a name='comments'>Comments</a>

  - Use [JSDoc](http://usejsdoc.org) notation for commenting guidelines.
  - Use `/** ... */` for multiline comments.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * Returns a new element based on
     * the passed in tag name.
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - Use `//` for single line comments. Always place single line comments on a newline above the subject of the comment. Put an empty line before the comment.

    ```javascript
    // bad
    var active = true;  // is current tab

    // good
    // is current tab
    var active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';i

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

  -  Always specify types and values for all parameters and return values. Ideally, provide a description for the function and parameters as well.

    ```javascript
    // bad
    function make(tag) {
      // ...stuff...
    }

    // good
    /**
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {
      // ...stuff...
    }

    // best
    /**
     * Returns a new element based on the passed in tag name.
     *
     * @param {String} tag
     *  Tag to create. -eg 'a', 'span', 'strong'
     * @return {Element} element
     *  DOM element object.
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

    **[[⬆]](#TOC)**


## <a name='whitespace'>Whitespace</a>

  - Use soft tabs set to 2 spaces

    ```javascript
    // bad
    function() {
    ∙∙∙∙var name;
    }

    // bad
    function() {
    ∙ var name;
    }

    // good
    function() {
    ∙∙var name;
    }
    ```
  - Place 1 space before the leading brace.

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```

    **[[⬆]](#TOC)**


## <a name='leading-commas'>Leading Commas</a>

  - **Nope.**

    ```javascript
    // bad
    var once
      , upon
      , aTime;

    // bad
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // good
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

    **[[⬆]](#TOC)**


## <a name='semicolons'>Semicolons</a>

  - **Yup.**

    ```javascript
    // bad
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // good
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // good
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    **[[⬆]](#TOC)**


## <a name='type-coercion'>Type Coercion</a>

  - Perform type coercion at the beginning of the statement.
  - Strings:

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    var totalScore = this.reviewScore + '';

    // good
    var totalScore = '' + this.reviewScore;

    // bad
    var totalScore = '' + this.reviewScore + ' total score';

    // good
    var totalScore = this.reviewScore + ' total score';
    ```

  - Numbers:
- Don't perform type coercion for integers. Use `parseInt` with a radix parameter instead.

    ```javascript
    var inputValue = '4';

    // bad
    var val = new Number(inputValue);

    // bad
    var val = +inputValue;

    // good
    var val = parseInt(inputValue, 10);
    ```

    **[[⬆]](#TOC)**


## <a name='naming-conventions'>Naming Conventions</a>

  - Avoid single letter names. Be descriptive with your naming. These get uglified anyways.

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

  - Use lower camelCase when naming objects, functions, and instances

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    var this-is-my-object = {};
    function c() {};
    var u = new user({
      name: 'Bob Parr'
    });

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {};
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - Use PascalCase when naming constructors or classes

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // good
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - Name your functions. This is helpful for stack traces.

    ```javascript
    // bad
    var log = function(msg) {
      console.log(msg);
    };

    // good
    var log = function log(msg) {
      console.log(msg);
    };
    ```

    **[[⬆]](#TOC)**


## <a name='accessors'>Accessors</a>

  - Accessor functions for properties are _not_ required
  - If you do make accessor functions, always prepend with `get` and `set`. Eg- getVal() and setVal('hello').

    ```javascript
    // bad
    dragon.age();

    // good
    dragon.getAge();

    // bad
    dragon.age(25);

    // good
    dragon.setAge(25);
    ```

  - If the property is a boolean, use isVal() or hasVal()

    ```javascript
    // bad
    if (!dragon.age()) {
      return false;
    }

    // good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - It's okay to create get() and set() functions, but be consistent.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function(key) {
      return this[key];
    };
    ```

    **[[⬆]](#TOC)**


## <a name='constructors'>Constructors</a>

  - Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // bad
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // good
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - Constructor methods should try to return `this`. This helps with method chaining which is often useful.

    ```javascript
    // bad
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20) // => undefined

    // good
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```

    **[[⬆]](#TOC)**


## <a name='jquery'>jQuery</a>

  - Prefix jQuery object variables with a `$`.

    ```javascript
    // bad
    var sidebar = $('.sidebar');

    // good
    var $sidebar = $('.sidebar');

    // bad
    var $primary_nav = $('#primary-nav');

    // good
    var $primaryNav = $('#primary-nav');
    ```

  - If a jQuery lookup is performed more than once, cache the jQuery object.

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // bad
    function getSideBarHeight() {
      var $sidebar = $('.sidebar');

      $sidebar.height();
    }

    // good
    function getSideBarHeight() {
      $('.sidebar').height();
    }

    // good
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - Scope jQuery object queries with find. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/13)

    ```javascript
    // bad
    $('.sidebar ul').hide();

    // bad
    $('.sidebar > ul').hide();

    // bad
    $('.sidebar', 'ul').hide();

    // good
    $('.sidebar').find('ul').hide();
    ```

  - Don't combine elements with a class or ID as part of the selector. If this results in a bad selection, you probably need to refactor your DOM.

    ```javascript
    // bad
    $('form#new_user').submit();

    // bad
    $('li.user').hide();

    // good
    $('#new_user').submit();

    // good
    $('.user').hide();
    ```

    **[[⬆]](#TOC)**


## <a name='modules'>Modules</a>

  - We use the JavaScript Module Pattern, as defined by Ben Cherry. [Read up on it here](http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html).
  - Files should be named with camelCase, live in their appropriate folder (views/ utils/ etc), and match the name of the single export.
  - Always declare `'use strict;'` at the top of the module.
  - Always return the original `module` object.
  - If you need to reference any other global objects, pass it to the module's arguments to help provide faster lookups and prevent linting errors.

    ```javascript
    /*jshint forin:true, noarg:true, noempty:true, eqeqeq:true, bitwise:true, strict:true, undef:true, unused:true, curly:true, browser:true, jquery:true, indent:2, maxerr:100 */

    /*
      @namespace viewName
      @memberOf haw.views
    */
    var haw = (function (module, $) {
      'use strict';

      // Module Namespace Extension
      var views = module.views = module.views || {};
      var viewName = views.viewName = views.viewName || {};


      // Private Variables
      var foo = "bar";

      // Private Method
      function privateMethod () {}

      // Public API
      viewName.publicMethod = function () {};

      // Return the extended module
      return module;

    }(haw || {}, jQuery));
    ```

    **[[⬆]](#TOC)**


## <a name='es5'>ECMAScript 5 Compatability</a>

  - Refer to [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.com/es5-compat-table/)

  **[[⬆]](#TOC)**


## <a name='performance'>Performance</a>

  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - Loading...

  **[[⬆]](#TOC)**


## <a name='resources'>Resources</a>


**Read This**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**Other Styleguides**

  - [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)

**Books**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch

**Blogs**

  - [DailyJS](//dailyjs.com)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [net.tutsplus](http://net.tutsplus.com/?s=javascript)

**Additional Articles**

  - [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) - Ben Cherry
  - [JavaScript Module Pattern: In-Depth](http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html) - Ben Cherry
  - [Named function expressions demystified](http://kangax.github.io/nfe/) - Juriy Zaytsev
  - [Function Declarations vs. Function Expressions](http://javascriptweblog.wordpress.com/2010/07/06/function-declarations-vs-function-expressions/) - Angus Croll
  - [Essential JavaScript Namespacing Patterns](http://addyosmani.com/blog/essential-js-namespacing/) - Addy Osmani

  **[[⬆]](#TOC)**

## <a name='guide-guide'>The JavaScript Style Guide Guide</a>

  - [Reference](//github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)


## <a name='license'>License</a>

(The MIT License)

Copyright (c) 2012 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[[⬆]](#TOC)**

