# JSX

---

* **什麼是 JSX？**

JSX 是一種在 Javascript 中使用的 XML 語法，目的是用來轉換成原生的 Javascript。React 官方推薦使用。  

* **為甚麼要用JSX?**

用原生 JS 寫起來會很麻煩 （一堆 createElement），因此，我們可以使用 JSX，它是 JS / ECMAScript 對 XML 的擴充語法，以 XML-like 的語法表達 JS 產生元件的函數，簡單來說就是語法糖 (Syntatic Sugar)。使用的話有助於精簡程式碼，而且對於大多數前端工程師來說 XML 的格式比較直觀，易於閱讀。  

```js
React.createElement('a', {href: 'https://facebook.github.io/react/'}, 'Hello!')  

等於

<a href="https://facebook.github.io/react/">Hello!</a>
```  

```js
// js
List l = new List();
Item i1 = new Item();
Item i2 = new Item();
l.Add(i1);
i.Add(i2);


// XML
<List>
  <Item name='i1' />
  <Item name='i2' />
</List>
```  

 * <font color='red'>**JSX 就是把 XML 語法轉換成 Javascript 的一種工具。**</font>  

* **JSX怎麼寫?**

HTML 大部份的寫法在 JSX 都可以通用，除了以下幾點限制：

1.HTML 的 class 屬性在 JSX 須寫為 className (class 為 JSX 保留字)
2.HTML 的 for 屬性在 JSX 須寫為 htmlFor (for 為 JSX 保留字)
3.所有 tag 都須被閉合 (XML 的特性)
  EX：HTML <br> => JSX <br />
4.同 JS，註解可以用 /* */ 或 //，在 tag 中使用的話則須用大括號 {} 包住
5.事件觸發是採用駝峰式命名法而不是全部小寫。  
    EX：
    ```js
    <button onClick={this.c8763}></button>
    ```
* 另外要特別要注意的是由於 JSX 最終會轉成 JavaScript 且每一個 JSX 節點都對應到一個 JavaScript 函數，所以在 Component 的 render 方法中只能回傳一個根節點（Root Nodes）。例如：若有多個 <tag> 要 render 請在外面包一個 Component 或 <div>、<span> 元素。  

