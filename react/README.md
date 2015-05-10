# Airbnb React/JSX Style Guide

*一份彙整了在 React 及 JSX 中被普遍使用的風格指南。*

<a name="table-of-contents"></a>
## 目錄

  1. [基本規範](#basic-rules)
  1. [命名](#naming)
  1. [宣告](#declaration)
  1. [對齊](#alignment)
  1. [引號](#quotes)
  1. [空格](#spacing)
  1. [Props](#props)
  1. [括號](#parentheses)
  1. [標籤](#tags)
  1. [方法](#methods)
  1. [排序](#ordering)

<a name="basic-rules"></a>
## 基本規範

  - 一個檔案只包含一個 React 元件。
  - 總是使用 JSX 語法。
  - 請別使用 `React.createElement` ，除非你從一個不轉換 JSX 的檔案初始化。

<a name="naming"></a>
## 命名

  - **副檔名**：React 元件的副檔名請使用 `js`。
  - **檔案名稱**：檔案名稱請使用帕斯卡命名法。例如：`ReservationCard.js`。
  - **參考命名規範**: React 元件請使用帕斯卡命名法，元件的實例則使用駝峰式大小寫：

    ```javascript
    // bad
    const reservationCard = require('./ReservationCard');

    // good
    const ReservationCard = require('./ReservationCard');

    // bad
    const ReservationItem = <ReservationCard />;

    // good
    const reservationItem = <ReservationCard />;
    ```

    **元件命名規範**：檔案名稱須和元件名稱一致。所以 `ReservationCard.js` 的參考名稱必須為 ReservationCard。但對於目錄的根元件請使用 index.js 作為檔案名稱，並使用目錄名作為元件的名稱。

    ```javascript
    // bad
    const Footer = require('./Footer/Footer.js')

    // bad
    const Footer = require('./Footer/index.js')

    // good
    const Footer = require('./Footer')
    ```

<a name="declaration"></a>
## 宣告
  - 不要使用 displayName 來命名元件，請使用參考來命名元件。

    ```javascript
    // bad
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
    });

    // good
    const ReservationCard = React.createClass({
      // stuff goes here
    });
    export default ReservationCard;
    ```

<a name="alignment"></a>
## 對齊
  - js 語法請遵循以下的對齊風格

    ```javascript
    // bad
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // good
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // 如果 props 適合放在同一行，就將它們放在同一行上
    <Foo bar="bar" />

    // 通常子元素必須進行縮排
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Spazz />
    </Foo>
    ```

<a name="quotes"></a>
## 引號
  - 總是在 JSX 的屬性使用雙引號（`"`），但是所有的 JavaScript 請使用單引號。

    ```javascript
    // bad
    <Foo bar='bar' />

    // good
    <Foo bar="bar" />

    // bad
    <Foo style={{ left: "20px" }} />

    // good
    <Foo style={{ left: '20px' }} />
    ```

<a name="spacing"></a>
## 空格
  - 總是在自身結尾標籤前加上一個空格。

    ```javascript
    // bad
    <Foo/>

    // very bad
    <Foo                 />

    // bad
    <Foo
     />

    // good
    <Foo />
    ```

<a name="props"></a>
## Props
  - 總是使用駝峰式大小寫命名 prop。

    ```javascript
    // bad
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // good
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

<a name="parentheses"></a>
## 括號
  - 當 JSX 的標籤有多行時請使用括號將它們包起來：

    ```javascript
    /// bad
    render() {
      return <MyComponent className="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // good
    render() {
      return (
        <MyComponent className="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // good, 當只有一行
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

<a name="tags"></a>
## 標籤
  - 沒有子標籤時總是使用自身結尾標籤。

    ```javascript
    // bad
    <Foo className="stuff"></Foo>

    // good
    <Foo className="stuff" />
    ```

  - 如果你的元件擁有多行屬性，結尾標籤請放在新的一行。

    ```javascript
    // bad
    <Foo
      bar="bar"
      baz="baz" />

    // good
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

<a name="methods"></a>
## 方法
  - react 元件的內部方法不要使用底線當作前綴。

    ```javascript
    // bad
    React.createClass({
      _onClickSubmit() {
        // do stuff
      }

      // other stuff
    });

    // good
    React.createClass({
      onClickSubmit() {
        // do stuff
      }

      // other stuff
    });
    ```

<a name="ordering"></a>
## 排序
  - 在 react 元件中的方法請遵循以下的排序法則：

  1. displayName
  2. mixins (as of React v0.13 mixins are deprecated)
  3. statics
  4. propTypes
  5. getDefaultProps
  6. getInitialState
  7. componentWillMount
  8. componentDidMount
  9. componentWillReceiveProps
  10. shouldComponentUpdate
  11. componentWillUpdate
  12. componentDidUpdate
  13. componentWillUnmount
  14. *clickHandlers or eventHandlers* like onClickSubmit() or onChangeDescription()
  15. *getter methods for render* like getSelectReason() or getFooterContent()
  16. *Optional render methods* like renderNavigation() or renderProfilePicture()
  17. render

**[⬆ 回到頂端](#table-of-contents)**
