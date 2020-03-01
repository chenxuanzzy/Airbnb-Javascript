# Airbnb React/JSX Style Guide

*一份彙整了在 React 及 JSX 中被普遍使用的風格指南。*

<a name="table-of-contents"></a>
## 目錄

  1. [基本規範](#basic-rules)
  1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
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
  1. [`isMounted`](#ismounted)

<a name="basic-rules"></a>
## 基本規範

  - 一個檔案只包含一個 React 元件。
    - 不過，一個檔案可以有多個 [Stateless 或 Pure Components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions)。eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - 總是使用 JSX 語法。
  - 請別使用 `React.createElement`，除非你初始化 app 的檔案不是 JSX。

## Class vs `React.createClass` vs stateless

  - 如果你有內部 state 及 / 或 refs，使用 `class extends React.Component` 勝於 `React.createClass`，除非你有一個非常好的理由才使用 mixins。eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md)

    ```javascript
    // bad
    const Listing = React.createClass({
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    });

    // good
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    }
    ```

    如果你不使用 state 或 refs，使用一般函式（不是箭頭函式）勝於 classes：

    ```javascript

    // bad
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // bad (因為箭頭函式沒有「name」屬性)
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // good
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
    ```

<a name="naming"></a>
## 命名

  - **副檔名**：React 元件的副檔名請使用 `jsx`。
  - **檔案名稱**：檔案名稱請使用帕斯卡命名法。例如：`ReservationCard.jsx`。
  - **參考命名規範**: React 元件請使用帕斯卡命名法，元件的實例則使用駝峰式大小寫。eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```javascript
    // bad
    import reservationCard from './ReservationCard';

    // good
    import ReservationCard from './ReservationCard';

    // bad
    const ReservationItem = <ReservationCard />;

    // good
    const reservationItem = <ReservationCard />;
    ```

  - **元件命名規範**：檔案名稱須和元件名稱一致。例如，`ReservationCard.jsx` 的參考名稱必須為 `ReservationCard`。但對於目錄的根元件請使用 `index.jsx` 作為檔案名稱，並使用目錄名作為元件的名稱：

    ```javascript
    // bad
    import Footer from './Footer/Footer';

    // bad
    import Footer from './Footer/index';

    // good
    import Footer from './Footer';
    ```


<a name="declaration"></a>
## 宣告

  - 不要使用 `displayName` 來命名元件。請使用參考來命名元件。

    ```javascript
    // bad
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
    });

    // good
    export default class ReservationCard extends React.Component {
    }
    ```

<a name="alignment"></a>
## 對齊

  - JSX 語法請遵循以下的對齊風格。eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

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
      <Quux />
    </Foo>
    ```

<a name="quotes"></a>
## 引號

  - 總是在 JSX 的屬性使用雙引號（`"`），但是所有的 JS 請使用單引號。eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes)

  > 為什麼？JSX 屬性[不能包含跳脫的引號](http://eslint.org/docs/rules/jsx-quotes)，所以雙引號可以更容易輸入像 `"don't"` 的連接詞。
  > 一般的 HTML 屬性通常也使用雙引號而不是單引號，所以 JSX 屬性借鏡了這個慣例。

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

  - Omit the value of the prop when it is explicitly `true`. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```javascript
    // bad
    <Foo
      hidden={true}
    />

    // good
    <Foo
      hidden
    />
    ```

<a name="parentheses"></a>
## 括號

  - 當 JSX 的標籤有多行時請使用括號將它們包起來：eslint: [`react/wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/wrap-multilines.md)

    ```javascript
    // bad
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

  - 沒有子標籤時總是使用自身結尾標籤。eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```javascript
    // bad
    <Foo className="stuff"></Foo>

    // good
    <Foo className="stuff" />
    ```

  - 如果你的元件擁有多行屬性，結尾標籤請放在新的一行。eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

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

  - Bind event handlers for the render method in the constructor. eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

    > Why? A bind call in the render path creates a brand new function on every single render.

    ```javascript
    // bad
    class extends React.Component {
      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />
      }
    }

    // good
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv} />
      }
    }
    ```

  - React 元件的內部方法不要使用底線當作前綴。

    ```javascript
    // bad
    React.createClass({
      _onClickSubmit() {
        // do stuff
      },

      // other stuff
    });

    // good
    class extends React.Component {
      onClickSubmit() {
        // do stuff
      }

      // other stuff
    }
    ```

<a name="ordering"></a>
## 排序

  - `class extends React.Component` 的排序：

  1. optional `static` methods
  1. `constructor`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers 或 eventHandlers* 像是 `onClickSubmit()` 或 `onChangeDescription()`
  1. *getter methods for `render`* 像是 `getSelectReason()` 或 `getFooterContent()`
  1. *Optional render methods* 像是 `renderNavigation()` 或 `renderProfilePicture()`
  1. `render`

  - How to define `propTypes`, `defaultProps`, `contextTypes`, etc...

    ```javascript
    import React, { PropTypes } from 'react';

    const propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
      }
    }

    Link.propTypes = propTypes;
    Link.defaultProps = defaultProps;

    export default Link;
    ```

  - `React.createClass` 的排序：eslint: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

  1. `displayName`
  1. `propTypes`
  1. `contextTypes`
  1. `childContextTypes`
  1. `mixins`
  1. `statics`
  1. `defaultProps`
  1. `getDefaultProps`
  1. `getInitialState`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers 或 eventHandlers* 像是 `onClickSubmit()` 或 `onChangeDescription()`
  1. *getter methods for `render`* 像是 `getSelectReason()` 或 `getFooterContent()`
  1. *Optional render methods* 像是 `renderNavigation()` 或 `renderProfilePicture()`
  1. `render`

## `isMounted`

  - 切勿使用`isMounted`。eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > 為什麼？[`isMounted` 是個 anti-pattern][anti-pattern]，在 ES6 classes 不可使用，並且被正式棄用。

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

**[⬆ 回到頂端](#table-of-contents)**
