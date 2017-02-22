# 認識React

---

* **什麼是 React？**  

React 是 facebook 開發的一個 JS 函式庫，負責產生與管理前端的 UI 。它並不是框架。  特色在virtual dom 跟 component 的概念  

 **Virtual DOM**

 我們要跟使用者在網頁上做一些互動，難免要做一些抓取或更新、操作 DOM 的動作

ex : 動態增加一個欄位、顯示 ajax 回來的訊息....等等

而這些動作在所有前端運作中可以說是最耗效能的，因為每次操作DOM的時候，頁面都必須要重繪一次  

```js
for (var i = 0; i < 10; i++)
{
  var a = document.createElement("a");
  a.innerHTML = arr[i];
  div.appendChild(a);
}
```

React 在與 DOM 之間的溝通也多了一層 Virtual DOM

Virtual DOM 不是真正的 DOM，是由 js 模擬出來的，

而Virtual DOM 也比原本要手動的 DocumentFragment 做的更為完美多了

他會去計算上一次 Virtual DOM 跟這一次的差異

然後<font color='red'>只把差異的部分處理後輸出到頁面的 DOM上</font>，這種運算法叫做 DOM diff

所有前端運算中成本最高的就是 DOM 的操作

所以有了 Virtual DOM 的幫忙之後，我們只需要在裡面進行邏輯運算

有關實際 DOM 的操作就交給 Virtual DOM 了

 **Component**  

React 的另一個特點是他使用了Component的概念，將網頁上的block變成一個個的Component

將網頁元素Componentization之後，最後再把Component組裝成一個網頁 (有點像拼圖)

每個Component都會有個 render 方法

而當Component的state被改變了，React 就會呼叫Component的 render 方法去重繪整個Component的 UI

Componentization的好處也有可重複使用與好維護的優點  

**建立Component方法**  

```js
// 第一種. 使用 ES6 的類別 (classes)
class TodoApp extends React.Component {
  render() {
    return <div>TodoApp</div>;
  }
}

// 第二種. 使用 React.createClass API，通常用於 ES5 中
const TodoApp = React.createClass({
  render() {
    return <div>TodoApp</div>;
  }
});


<div id = 'app'></div>

ReactDOM.render(
<TodoApp />,
document.getElementById('div')
)
```

* **每個Component命名開頭一定要大寫**  

## **props & states**  

### **props**  

props 是 React 父子component間溝通的橋樑。靜態（唯讀）。
父component用屬性賦值的方式傳給子component，子component用 this.props 讀取。但不應於子component內變動 （唯讀）。
父component傳入的 props 改變將造成子component重繪。  

```js
// 父Component
...
<Child name="thomas"/>
...
```

```js
// 子Component  Child
...
<p>{this.props.name}</p>
...
```

**props 傳遞地獄**

當 Component 愈疊愈多層，想要把資料從最頂層的Component傳到最底層的Component就會愈麻煩，就是所謂的 props 傳遞地獄。
解決方法是使用後端框架 (EX: Redux) 將 state 集中起來，以便管理。

### **this.refs**

React 支持一種非常特殊的屬性 Ref ，你可以用來绑定到 render() 輸出的任何组件上。

this.refs 顧名思義就是參考的意思，有點像dom的id的意思  

```js
 var MyComponent = React.createClass({
      handleClick: function() {
        this.refs.myInput.focus();
      },
      render: function() {
        return (
          <div>
            <input type="text" ref="myInput" />
            <input
              type="button"
              value="點我獲取焦點"
              onClick={this.handleClick}
            />
          </div>
        );
      }
    });
    ReactDOM.render(
      <MyComponent />,
      document.getElementById('example')
    );
```

* 你可以定義任何public method在你的Component裡，比如 this.refs.myInput.focus()
* Refs 是訪問到Component内部 DOM node唯一可靠的方法
* 當子组件删除時,Refs 會自動銷毀對子组件的reference


### **List & Key**

```js
const numbers = [1, 2, 3, 4, 5];

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li>{number}</li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('app')
);
```
當您運行此代碼時，將會收到一條警告
**<font color='red'>"Warning: Each child in an array or iterator should have a unique \"key\" prop. Check the render method of `NumberList`. See https://fb.me/react-warning-keys for more information."</font>**

讓我們分配一個key到我們的列表項裡面numbers.map()並修復缺少的key問題。  

```js
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('app')
);
```

通常，您會使用數據中的ID作為鍵：

```js
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```

當您對已呈現的項目沒有穩定的ID時，可以使用項目索引作為最後一個手段的鍵：  

```js
const todoItems = todos.map((todo, index) =>
  <li key={index}>
    {todo.text}
  </li>
);
```

如果項目可以重新排序，我們不建議對鍵使用索引，因為這將很慢。

### **states**  

states 是元件內部狀態。動態（可用 setState 改值）。
與一般變數不同的是，它無法直接修改（初始化例外），只能用 this.setState() 修改。
每次使用 this.setState() 修改 state 都會造成元件重繪。  

```js
// state 初始化
class TodoApp extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
        users: []
      };
    };
    //render() 必須覆寫，並回傳 JSX tag /null/false
   render() {
      return null;
    }
  }
```

```js
// setState
this.setState({
  users: list
});
```

在預設的情況下，從父Component傳值到子Component之後，子元件會使用 this.props 來做接值使用

每一個Component都會根據自己的 props 來 render 自己一次，但是因為 props 都是從父元件來的

所以 props 是不能更改的，此時就要使用 state 來達到互動的目的

使用 state 之後工作方式會變成這樣：

我們讓Component一開始有一個初始的狀態，然後隨著使用者的互動會讓狀態改變，此時就會觸發讓 UI 重繪 (render) 就會達到我們雙向溝通的目的  

```js
var LikeButton = React.createClass({
  getInitialState: function() {
    return {liked: false};
  },
  handleClick: function(event) {
    this.setState({liked: !this.state.liked});
  },
  render: function() {
      var text = this.state.liked ? 'like' : 'haven\'t liked';
      return (
      <p onClick={this.handleClick}>
      You {text} this. Click to toggle.
      </p>
    );
  }
});

ReactDOM.render(
<LikeButton />,
document.getElementById('example')
);
```

**State 使用的時機：**

state 主要是用在要與使用者互動的地方，像頁面上輸入的文字方塊，或需要動態呈現 ajax 回來後的資料

而太頻繁的使用 state 代表可能需要承擔元件 render 後出現錯誤的風險

所以如果網頁不需要與使用者互動就不要使用 state，然後如果有邏輯的運算不要放在 state 中

盡量在 render 裡面做掉，可以更確保UI與數據的一致性