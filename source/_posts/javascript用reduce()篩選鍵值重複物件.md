---
title: JavaScript 用 reduce() 方法篩選鍵值重複的陣列物件
tags: 
- JavaScript 
- method
- reduce()
- ES6
- 陣列
- 物件
- 展開運算子
abbrlink: 0228
date: 2021-02-28 20:10:05
---


 ## reduce() 方法

reduce() 會將 return 的值儲存在 accumulator 並依照陣列的長度決定執行的次數，並會回傳一個值，主要的使用場景多為相加一組陣列中的數值，參數如下。

```javascript
     reduce(callback[accumulator, currentValue, currentIndex, array], initialValue)
```

**accumulator** reduce的主角，用來記錄return的值
**currentValue** 當前操作中的值
**currentIndex** 當前操作值的索引
**array** 操作的陣列本身
**initialValue** 設定 accumulator 的初始值，可選填

currentIndex 跟 array 筆者還沒查到好用的用法，本文先不討論。

---


### 相加陣列數值：

```javascript
const arr = [1, 2, 2, 3, 3, 4]
const result = arr.reduce((acc, cur) => acc + cur)
console.log(result)
// result = 15
```

打開 F12 的 Console 測試看看，印出來的值應為 15，中間發生了什麼神奇的事情呢?
在函式中加入 console.log(acc) 來測試看看，會得到 1 3 5 8 11 15，當中 acc 每次都會印出上次操作時回傳的值，接下來我們可以用這個特性，來做陣列重複值的篩選。


---

### 過濾重複的值：

```javascript
const arr = [1, 2, 2, 3, 3, 4]
const result = arr.reduce((acc, cur) => {
     acc[cur] = cur
     return acc
}, {})
console.log(Object.values(result))
// result = [1, 2, 3, 4]
```

一樣在函式中加入 console.log(acc) 來測試看看，這裡直接把初始的 acc 的 initialValue 設定為一個空物件，並透過 acc[cur] = cur 直接用 cur 查找到的 key 對應的屬性值設為 cur 。

如果 cur 目前為 1，找到 key = 1 的屬性, 並設定value = 1，acc為 {1: 1}。
如果 cur 目前為 2，找到 key = 2 的屬性, 並設定value = 2，acc為: {1: 1, 2: 2}。
這樣查找到重複的 key 屬性就會被複寫，不會產生重複的屬性，全部操作一遍以後，再用 Object.values() 方法取出物件中的 value，組成一個不重複值的陣列。

---

### 過濾重複值的物件

介紹完數字陣列，現在來操作過濾物件陣列。

```javascript
const arr = [
     { id: 1, name: 'Ken' },
     { id: 1, name: 'Ken' },
     { id: 2, name: 'Joshua' },
     { id: 3, name: 'Shane' },
     { id: 3, name: 'Shane' },
     { id: 4, name: 'Diva' }
]
const result = arr.reduce((acc, cur) => acc.find(item => item.id === cur.id) ? acc : [...acc, cur], [])
console.log(result)
```

> <s>**配上ES6的語法，就看不懂這段在幹嘛了呢，真棒。**</s>

我們首先來看，先將 initialValue 設定為空陣列，並在累加過程中對 acc 當中的物件做 find()，因為 find() 方法只會回傳一個結果，那麼只要沒有 id 重複的物件，就會回傳一個 undefined ，並 return 一個加入 cur 的陣列，另外 [...acc, cur] 是展開運算子，用來展開 acc 陣列，並加入 cur 到一個空的陣列中。

結果：

```javascript
result = [
     { id: 1, name: 'Ken' },
     { id: 2, name: 'Joshua' },
     { id: 3, name: 'Shane' },
     { id: 4, name: 'Diva' }
]
```

---

### 結語

其實 reduce() 操作上個人認為比較不直觀，因為這個方法的疊加器觀念比較進階，對初學者可能比較不友善， 而且 reduce() 本身推薦的運用場景是拿來做陣列值累加或相減，如果要想更改篩選或比較當中的邏輯，可能需要先花比較多時間理解。
