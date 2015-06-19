[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/airbnb/javascript?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

# Airbnb JavaScript Style Guide() {


*一份彙整了在 JavasScript 中被普遍使用的風格指南。*

翻譯自 [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) 。


<a name="table-of-contents"></a>
## 目錄

  1. [資料型態](#types)
  1. [物件](#objects)
  1. [陣列](#arrays)
  1. [字串](#strings)
  1. [函式](#functions)
  1. [屬性](#properties)
  1. [變數](#variables)
  1. [提升](#hoisting)
  1. [條件式與等號](#conditional-expressions--equality)
  1. [區塊](#blocks)
  1. [註解](#comments)
  1. [空格](#whitespace)
  1. [逗號](#commas)
  1. [分號](#semicolons)
  1. [型別轉換](#type-casting--coercion)
  1. [命名規則](#naming-conventions)
  1. [存取器](#accessors)
  1. [建構子](#constructors)
  1. [事件](#events)
  1. [模組](#modules)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 相容性](#ecmascript-5-compatibility)
  1. [測試](#testing)
  1. [效能](#performance)
  1. [資源](#resources)
  1. [誰在使用](#in-the-wild)
  1. [翻譯](#translation)
  1. [JavaScript 風格指南](#the-javascript-style-guide-guide)
  1. [和我們討論 Javascript](#chat-with-us-about-javascript)
  1. [貢獻者](#contributors)
  1. [授權許可](#license)

<a name="types"></a>
## 資料型態

  - **基本**: 你可以直接存取基本資料型態。

    + `字串`
    + `數字`
    + `布林`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1;
    var bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **複合**: 你需要透過引用的方式存取複合資料型態。

    + `物件`
    + `陣列`
    + `函式`

    ```javascript
    var foo = [1, 2];
    var bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="objects"></a>
## 物件

  - 使用簡潔的語法建立物件。

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```

  - 別使用[保留字](http://es5.github.io/#x7.6.1)當作鍵值，他在 IE8 上不會被執行。[了解更多](https://github.com/airbnb/javascript/issues/61)

    ```javascript
    // bad
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // good
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - 使用同義詞取代保留字。

    ```javascript
    // bad
    var superman = {
      class: 'alien'
    };

    // bad
    var superman = {
      klass: 'alien'
    };

    // good
    var superman = {
      type: 'alien'
    };
    ```


**[⬆ 回到頂端](#table-of-contents)**

<a name="arrays"></a>
## 陣列

  - 使用簡潔的語法建立陣列。

    ```javascript
    // bad
    var items = new Array();

    // good
    var items = [];
    ```

  - 如果你不知道陣列的長度請使用 Array#push。

    ```javascript
    var someStack = [];


    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  - 如果你要複製一個陣列請使用 Array#slice。[jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length;
    var itemsCopy = [];
    var i;

    // bad
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    itemsCopy = items.slice();
    ```

  - 如果要轉換一個像陣列的物件至陣列，可以使用 Array#slice。

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="strings"></a>
## 字串

  - 字串請使用單引號 `''` 。

    ```javascript
    // bad
    var name = "Bob Parr";

    // good
    var name = 'Bob Parr';

    // bad
    var fullName = "Bob " + this.lastName;

    // good
    var fullName = 'Bob ' + this.lastName;
    ```

  - 如果字串超過 80 個字元，請使用字串連接符號 `\` 換行。
  - 注意: 過度的長字串連接可能會影響效能 [jsPerf](http://jsperf.com/ya-string-concat) 與[討論串](https://github.com/airbnb/javascript/issues/40)。

    ```javascript
    // bad
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    var errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // good
    var errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  - 如果要透過陣列產生字串，請使用 Array#join 代替字串連接符號，尤其是 IE：[jsPerf](http://jsperf.com/string-vs-array-concat/2)。

    ```javascript
    var items;
    var messages;
    var length;
    var i;

    messages = [{
      state: 'success',
      message: 'This one worked.'
    }, {
      state: 'success',
      message: 'This one worked as well.'
    }, {
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
        // 在這個狀況時我們可以直接賦值來稍微最佳化程式
        items[i] = '<li>' + messages[i].message + '</li>';
      }

      return '<ul>' + items.join('') + '</ul>';
    }
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="functions"></a>
## 函式

  - 函式表達式:

    ```javascript
    // 匿名函式
    var anonymous = function() {
      return true;
    };

    // 命名函式
    var named = function named() {
      return true;
    };

    // 立即函式 (immediately-invoked function expression, IIFE)
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - 絕對不要在非函式的區塊（if, while, 等等）宣告函式，瀏覽器或許會允許你這麼做，但不同瀏覽器產生的結果可能會不同。你可以將函式賦予一個區塊外的變數解決這個問題。
  - **注意：**ECMA-262 將 `區塊` 定義為陳述式，函式宣告則不是陳述式。 [閱讀 ECMA-262 關於這個問題的說明](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97)。

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    var test;
    if (currentUser) {
      test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - 請勿將參數命名為 `arguments` ，這樣會將覆蓋掉函式作用域傳來的 `arguments` 。

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

**[⬆ 回到頂端](#table-of-contents)**

<a name="properties"></a>
## 屬性

  - 使用點 `.` 來存取屬性。

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

  - 需要帶參數存取屬性時請使用中括號 `[]` 。

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="variables"></a>
## 變數

  - 為了避免污染全域的命名空間，請使用 `var` 來宣告變數，如果不這麼做將會產生全域變數。

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    var superPower = new SuperPower();
    ```

  - 每個變數只使用一個 `var` 來宣告，這樣更容易增加新的變數宣告，而且你也不用擔心替換 `;` 為 `,` 及加入的標點符號不同的問題。

    ```javascript
    // bad
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    // （比較上述例子找出錯誤）
    var items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

  - 將未賦值的變數宣告在最後，當你需要根據之前已賦值變數來賦值給未賦值變數時相當有幫助。

    ```javascript
    // bad
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    var i;
    var items = getItems();
    var dragonball;
    var goSportsTeam = true;
    var len;

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball;
    var length;
    var i;
    ```

  - 在作用域的最頂層宣告變數，避免變數宣告及賦值提升的相關問題。

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

    // bad - 呼叫多餘的函式
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      this.setFirstName(name);

      return true;
    }

    // good
    function() {
      var name;

      if (!arguments.length) {
        return false;
      }

      name = getName();
      this.setFirstName(name);

      return true;
    }
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="hoisting"></a>
## 提升

  - 變數宣告可以被提升至該作用域的最頂層，但賦予的值並不會。

    ```javascript
    // 我們知道這樣是行不通的
    // （假設沒有名為 notDefined 的全域變數）
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // 由於變數提升的關係，
    // 你在引用變數後再宣告變數是行得通的。
    // 注：賦予給變數的 `true` 並不會被提升。
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // 直譯器會將宣告的變數提升至作用域的最頂層，
    // 表示我們可以將這個例子改寫成以下：
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - 賦予匿名函式的變數會被提升，但函式內容並不會。

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - 賦予命名函式的變數會被提升，但函式內容及函式名稱並不會。

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // 當函式名稱和變數名稱相同時也是如此。
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - 宣告函式的名稱及函式內容都會被提升。

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - 想瞭解更多訊息，請參考 [Ben Cherry](http://www.adequatelygood.com/) 的 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting)。

**[⬆ 回到頂端](#table-of-contents)**

<a name="conditional-expressions--equality"></a>
## 條件式與等號

  - 請使用 `===` 和 `!==` ，別使用 `==` 及 `!=` 。
  - 像是 `if` 的條件語法內會使用 `ToBoolean` 的抽象方法強轉類型，並遵循以下規範：

    + **物件** 轉換為 **true**
    + **Undefined** 轉換為 **false**
    + **Null** 轉換為 **false**
    + **布林** 轉換為 **該布林值**
    + **數字** 如果是 **+0, -0, 或 NaN** 則轉換為 **false** ，其他的皆為 **true**
    + **字串** 如果是空字串 `''` 則轉換為 **false** ，其他的皆為 **true**

    ```javascript
    if ([0]) {
      // true
      // 陣列為一個物件，所以轉換為 true
    }
    ```

  - 使用快速的方式。

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (collection.length > 0) {
      // ...stuff...
    }

    // good
    if (collection.length) {
      // ...stuff...
    }
    ```

  - 想瞭解更多訊息請參考 Angus Croll 的 [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108)。

**[⬆ 回到頂端](#table-of-contents)**

<a name="blocks"></a>
## 區塊

  - 多行區塊請使用花括號括起來。

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function() { return false; }

    // good
    function() {
      return false;
    }
    ```

    - 如果你使用 `if` 及 `else` 的多行區塊，請將 `else` 放在 `if` 區塊的結尾花括號之後。

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="comments"></a>
## 註解

  - 多行註解請使用 `/** ... */` ，包含描述，指定類型以及參數值還有回傳值。

    ```javascript
    // bad
    // make() 根據傳入的 tag 名稱回傳一個新的元件
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() 根據傳入的 tag 名稱回傳一個新的元件
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - 單行註解請使用 `//` ，在欲註解的地方上方進行當行註解，並在註解前空一格。

    ```javascript
    // bad
    var active = true;  // 當目前分頁

    // good
    // 當目前分頁
    var active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // 設定預設的類型為 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // 設定預設的類型為 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

  - 在註解前方加上 `FIXME` 或 `TODO` 可以幫助其他開發人員快速瞭解這是一個需要重新討論的問題，或是一個等待解決的問題。和一般的註解不同，他們是可被執行的。對應的動作為 `FIXME -- 重新討論並解決` 或 `TODO -- 必須執行`.

  - 使用 `// FIXME:` 標注問題。

    ```javascript
    function Calculator() {

      // FIXME: 不該在這使用全域變數
      total = 0;

      return this;
    }
    ```

  - 使用 `// TODO:` 標注問題的解決方式

    ```javascript
    function Calculator() {

      // TODO: total 應該可被傳入的參數所修改
      this.total = 0;

      return this;
    }
  ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="whitespace"></a>
## 空格

  - 將 Tab 設定為兩個空格。

    ```javascript
    // bad
    function() {
    ∙∙∙∙var name;
    }

    // bad
    function() {
    ∙var name;
    }

    // good
    function() {
    ∙∙var name;
    }
    ```

  - 在花括號前加一個空格。

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

  - 在控制流程的語句（`if`, `while` 等等。）的左括號前加上一個空格。宣告的函式和傳入的變數間則沒有空格。

    ```javascript
    // bad
    if(isJedi) {
      fight ();
    }

    // good
    if (isJedi) {
      fight();
    }

    // bad
    function fight () {
      console.log ('Swooosh!');
    }

    // good
    function fight() {
      console.log('Swooosh!');
    }
    ```

  - 將運算元用空格隔開。

    ```javascript
    // bad
    var x=y+5;

    // good
    var x = y + 5;
    ```

  - 在檔案的最尾端加上一行空白行。

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function(global) {
      // ...stuff...
    })(this);↵
    ```

  - 當多個方法連接時請換行縮排，利用前面的 `.` 強調該行是呼叫方法，而不是一個新的宣告。

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // bad
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - 區塊的結束和下個語法間加上空行。

    ```javascript
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;

    // bad
    var obj = {
      foo: function() {
      },
      bar: function() {
      }
    };
    return obj;

    // good
    var obj = {
      foo: function() {
      },

      bar: function() {
      }
    };

    return obj;
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="commas"></a>
## 逗號

  - 不要將逗號放在前方。

    ```javascript
    // bad
    var story = [
        once
      , upon
      , aTime
    ];

    // good
    var story = [
      once,
      upon,
      aTime
    ];

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

  - 多餘的逗號：**Nope.** 在 IE6/7 及 IE9 的相容性模式中，多餘的逗號可能會產生問題。另外，在 ES3 的一些實現方式上會多計算陣列的長度，不過在 ES5 中已經被修正了（[來源](http://es5.github.io/#D)）：

  > Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

    ```javascript
    // bad
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // good
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="semicolons"></a>
## 分號

  - 句尾請加分號。

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

    // good （防止當兩個檔案含有立即函式需要合併時，函式被當成參數處理）
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    [瞭解更多](http://stackoverflow.com/a/7365214/1712802).

**[⬆ 回到頂端](#table-of-contents)**

<a name="type-casting--coercion"></a>
## 型別轉換

  - 在開頭的宣告進行強制型別轉換。
  - 字串：

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

  - 對數字使用 `parseInt` 轉換，並帶上型別轉換的基數。

    ```javascript
    var inputValue = '4';

    // bad
    var val = new Number(inputValue);

    // bad
    var val = +inputValue;

    // bad
    var val = inputValue >> 0;

    // bad
    var val = parseInt(inputValue);

    // good
    var val = Number(inputValue);

    // good
    var val = parseInt(inputValue, 10);
    ```

  - 如果你因為某個原因在做些瘋狂的事情，但是 `parseInt` 是你的瓶頸，所以你對於[性能方面的原因](http://jsperf.com/coercion-vs-casting/3)而必須使用位元右移，請留下評論並解釋為什麼使用，及你做了哪些事情。

    ```javascript
    // good
    /**
    * 使用 parseInt 導致我的程式變慢，改成使用
    * 位元右移強制將字串轉為數字加快了他的速度。
    */
    var val = inputValue >> 0;
    ```

  - **注意：**使用位元轉換時請小心，數字為 [64 位元數值](http://es5.github.io/#x4.3.19)，但是使用位元轉換時則會回傳一個 32 位元的整數 （[來源](http://es5.github.io/#x11.7)），這會導致大於 32 位元的數值產生異常[討論串](https://github.com/airbnb/javascript/issues/109)，32 位元的整數最大值為 2,147,483,647：

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - 布林:

    ```javascript
    var age = 0;

    // bad
    var hasAge = new Boolean(age);

    // good
    var hasAge = Boolean(age);

    // good
    var hasAge = !!age;
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="naming-conventions"></a>
## 命名規則

  - 避免使用單一字母的名稱，讓你的名稱有解釋的含義。

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

  - 使用駝峰式大小寫命名物件，函式及實例。

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    var o = {};
    function c() {}

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  - 使用帕斯卡命名法來命名建構函式或類別。

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

  - 命名私有屬性時請在前面加底線 `_`。

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

  - 要保留對 `this` 的使用時請用 `_this` 取代。

    ```javascript
    // bad
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }

    // bad
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // good
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }
    ```

  - 將你的函式命名，這對於在做堆疊追蹤時相當有幫助。

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

  - **注意:**IE8 及 IE8 以下對於命名函式有獨到見解。更多的訊息在 [http://kangax.github.io/nfe/](http://kangax.github.io/nfe/)。

  - 如果你的檔案只有輸出一個類別，你的檔案名稱必須和你的類別名稱相同。
    ```javascript
    // 檔案內容
    class CheckBox {
      // ...
    }
    module.exports = CheckBox;

    // 在其他的檔案
    // bad
    var CheckBox = require('./checkBox');

    // bad
    var CheckBox = require('./check_box');

    // good
    var CheckBox = require('./CheckBox');
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="accessors"></a>
## 存取器

  - 存取器不是必須的。
  - 如果你要建立一個存取器，請使用 getVal() 及 setVal('hello')。

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

  - 如果屬性是布林，請使用 isVal() 或 hasVal()。

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

  - 可以建立 get() 及 set() 函式，但請保持一致。

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

**[⬆ 回到頂端](#table-of-contents)**

<a name="constructors"></a>
## 建構子

  - 將方法分配給物件原型，而不是用新的物件覆蓋掉原型，否則會導致繼承出現問題：重置原型時你會覆蓋原有的原型。

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

  - 方法可以回傳 `this` 幫助方法鏈接。

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
    luke.setHeight(20); // => undefined

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


  - 可以寫一個 toString() 的方法，但是請確保他可以正常執行且沒有函式副作用。

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="events"></a>
## 事件

  - 當需要對事件傳入資料時（不論是 DOM 事件或是其他私有事件），請傳入物件替代單一的資料。這樣可以使之後的開發人員直接加入其他的資料到事件裡，而不需更新該事件的處理器。例如，比較不好的做法：

    ```js
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    更好的做法：

    ```js
    // good
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  **[⬆ 回到頂端](#table-of-contents)**

<a name="modules"></a>
## 模組

  - 模組的開頭必須以 `!` 開頭， 這樣可以確保前一模組結尾忘記加分號時在合併後不會出現錯誤。[說明](https://github.com/airbnb/javascript/issues/44#issuecomment-13063933)
  - 命名方式請使用駝峰式大小寫，並存在同名的資料夾下，導出時的名稱也必須一致。
  - 加入一個名稱為 `noConflict()` 方法來設置導出時的模組為前一個版本，並將他回傳。
  - 記得在模組的最頂端加上 `'use strict';` 。

    ```javascript
    // fancyInput/fancyInput.js

    !function(global) {
      'use strict';

      var previousFancyInput = global.FancyInput;

      function FancyInput(options) {
        this.options = options || {};
      }

      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
        return FancyInput;
      };

      global.FancyInput = FancyInput;
    }(this);
    ```

**[⬆ 回到頂端](#table-of-contents)**


## jQuery

  - jQuery 的物件請使用 `$` 當前綴。

    ```javascript
    // bad
    var sidebar = $('.sidebar');

    // good
    var $sidebar = $('.sidebar');
    ```

  - 快取 jQuery 的查詢。

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
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

  - DOM 的查詢請使用層遞的 `$('.sidebar ul')` 或 父元素 > 子元素 `$('.sidebar > ul')` 。[jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - 對作用域內的 jQuery 物件使用 `find` 做查詢。

    ```javascript
    // bad
    $('ul', '.sidebar').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="ecmascript-5-compatibility"></a>
## ECMAScript 5 相容性

  - 參考 [Kangax](https://twitter.com/kangax/) 的 ES5 [相容性列表](http://kangax.github.com/es5-compat-table/).

**[⬆ 回到頂端](#table-of-contents)**

<a name="testing"></a>
## 測試

  - **如題。**

    ```javascript
    function() {
      return true;
    }
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="performance"></a>
## 效能

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

**[⬆ 回到頂端](#table-of-contents)**

<a name="resources"></a>
## 資源


**請讀這個**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**工具**

  - Code Style Linters
    + [JSHint](http://www.jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/jshintrc)
    + [JSCS](https://github.com/jscs-dev/node-jscs) - [Airbnb Style Preset](https://github.com/jscs-dev/node-jscs/blob/master/presets/airbnb.json)

**其他的風格指南**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)

**其他風格**

  - [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
  - [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**瞭解更多**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban
  - [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

**書籍**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/) - Julien Bouquillon
  - [Third Party JavaScript](http://manning.com/vinegar/) - Ben Vinegar and Anton Kovalyov
  - [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](http://amzn.com/0321812182) - David Herman
  - [Eloquent JavaScript](http://eloquentjavascript.net) - Marijn Haverbeke
  - [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS) - Kyle Simpson

**部落格**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

**Podcasts**

  - [JavaScript Jabber](http://devchat.tv/js-jabber/)


**[⬆ 回到頂端](#table-of-contents)**

<a name="in-the-wild"></a>
## 誰在使用

  這是正在使用這份風格指南的組織列表。送一個 pull request 或提一個 issue 讓我們將你增加到列表上。

  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Adult Swim**: [adult-swim/javascript](https://github.com/adult-swim/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **American Insitutes for Research**: [AIRAST/javascript](https://github.com/AIRAST/javascript)
  - **Apartmint**: [apartmint/javascript](https://github.com/apartmint/javascript)
  - **Avalara**: [avalara/javascript](https://github.com/avalara/javascript)
  - **Billabong**: [billabong/javascript](https://github.com/billabong/javascript)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **DailyMotion**: [dailymotion/javascript](https://github.com/dailymotion/javascript)
  - **Digitpaint** [digitpaint/javascript](https://github.com/digitpaint/javascript)
  - **Evernote**: [evernote/javascript-style-guide](https://github.com/evernote/javascript-style-guide)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Flexberry**: [Flexberry/javascript-style-guide](https://github.com/Flexberry/javascript-style-guide)
  - **Gawker Media**: [gawkermedia/javascript](https://github.com/gawkermedia/javascript)
  - **GeneralElectric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript)
  - **InfoJobs**: [InfoJobs/JavaScript-Style-Guide](https://github.com/InfoJobs/JavaScript-Style-Guide)
  - **Intent Media**: [intentmedia/javascript](https://github.com/intentmedia/javascript)
  - **Jam3**: [Jam3/Javascript-Code-Conventions](https://github.com/Jam3/Javascript-Code-Conventions)
  - **JSSolutions**: [JSSolutions/javascript](https://github.com/JSSolutions/javascript)
  - **Kinetica Solutions**: [kinetica/javascript](https://github.com/kinetica/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **Money Advice Service**: [moneyadviceservice/javascript](https://github.com/moneyadviceservice/javascript)
  - **Muber**: [muber/javascript](https://github.com/muber/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **Nimbl3**: [nimbl3/javascript](https://github.com/nimbl3/javascript)
  - **Nordic Venture Family**: [CodeDistillery/javascript](https://github.com/CodeDistillery/javascript)
  - **Orion Health**: [orionhealth/javascript](https://github.com/orionhealth/javascript)
  - **Peerby**: [Peerby/javascript](https://github.com/Peerby/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **reddit**: [reddit/styleguide/javascript](https://github.com/reddit/styleguide/tree/master/javascript)
  - **REI**: [reidev/js-style-guide](https://github.com/reidev/js-style-guide)
  - **Ripple**: [ripple/javascript-style-guide](https://github.com/ripple/javascript-style-guide)
  - **SeekingAlpha**: [seekingalpha/javascript-style-guide](https://github.com/seekingalpha/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **StudentSphere**: [studentsphere/javascript](https://github.com/studentsphere/javascript)
  - **Target**: [target/javascript](https://github.com/target/javascript)
  - **TheLadders**: [TheLadders/javascript](https://github.com/TheLadders/javascript)
  - **T4R Technology**: [T4R-Technology/javascript](https://github.com/T4R-Technology/javascript)
  - **Userify**: [userify/javascript](https://github.com/userify/javascript)
  - **VoxFeed**: [VoxFeed/javascript-style-guide](https://github.com/VoxFeed/javascript-style-guide)
  - **Weggo**: [Weggo/javascript](https://github.com/Weggo/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

<a name="translation"></a>
## 翻譯

  這份風格指南也提供其他語言的版本：

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - ![bg](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bulgaria.png) **Bulgarian**: [borislavvv/javascript](https://github.com/borislavvv/javascript)
  - ![ca](https://raw.githubusercontent.com/fpmweb/javascript-style-guide/master/img/catala.png) **Catalan**: [fpmweb/javascript-style-guide](https://github.com/fpmweb/javascript-style-guide)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese(Traditional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese(Simplified)**: [sivan/javascript-style-guide](https://github.com/sivan/javascript-style-guide)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [sinkswim/javascript-style-guide](https://github.com/sinkswim/javascript-style-guide)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javacript-style-guide](https://github.com/mitsuruog/javacript-style-guide)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [mjurczyk/javascript](https://github.com/mjurczyk/javascript)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [uprock/javascript](https://github.com/uprock/javascript)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Thai**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide)

<a name="the-javascript-style-guide-guide"></a>
## JavaScript 風格指南

  - [參考](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

<a name="chat-with-us-about-javascript"></a>
## 與我們討論 JavaScript

  - 請到 [gitter](https://gitter.im/airbnb/javascript) 尋找我們.

<a name="contributors"></a>
## 貢獻者

  - [查看貢獻者](https://github.com/airbnb/javascript/graphs/contributors)

<a name="license"></a>
## 授權許可

(The MIT License)

Copyright (c) 2014 Airbnb

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

**[⬆ 回到頂端](#table-of-contents)**

# };
