---
title: JavaScript ES6 與 reduce() 方法篩選鍵值重複的陣列物件
tags: 
- JavaScript 
- method
- reduce()
- ES6
- 展開運算子
abbrlink: 0228
date: 2021-02-28 20:10:05
---



 ## reduce() 方法

reduce() 的使用場景多為相加一組陣列中的數值，並會回傳一個值，參數如下

     reduce(callback[accumulator, currentValue, currentIndex, array], initialValue)

**accumulator** 經由 currentValue 累加的值
**currentValue** 當前操作中的值
**currentIndex** 當前的索引值
**array** 操作的陣列本身
**initialValue** 設定 accumulator 的初始值，可選填

currentIndex 跟 array 筆者還沒查到好用的用法，本文先不討論

---


### 相加陣列數值：

     const arr = [1, 2, 2, 3, 3, 4]
     const result = arr.reduce((acc, cur) => acc + cur)
     console.log(result)

     // result = 15

打開 F12 的 Console 測試看看，印出來的值應為 15
所以這個神奇的過程是什麼?
在函式中加入 console.log(acc) 來測試看看
會得到1 3 5 8 11 15
當中 acc 每次都會印出上次回傳的值
我們可以用這個特性，來做陣列重複值得篩選


---

### 過濾重複的值：

     const arr = [1, 2, 2, 3, 3, 4]
     const result = arr.reduce((acc, cur) => {
          acc[cur] = cur
          return acc
     }, {})
     console.log(Object.values(result))

     // result = [1, 2, 3, 4]

一樣在函式中加入 console.log(acc) 來測試看看
這裡直接把初始的 acc 的 initialValue 設定為一個空物件
並透過 acc[cur] = cur 直接用 cur 查找到的 key 對應的屬性值設為 cur 

假設 cur 目前為 1
得到 key 為 1 的 value 為 1
輸出: {1: 1}

假設 cur 目前為 2
得到 key 為 2 的 value 為 2
輸出: {1: 1, 2: 2}

這樣查找到重複的 key 就會直接複寫，不會產生重複的屬性
全部操作一遍以後，再用Object.values()方法取出值，組成一個不重複值的陣列

---

### 過濾重複值的物件

介紹完數字陣列，現在來操作過濾物件陣列

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


**配上ES6的語法，就看不懂這段在幹嘛了呢，真棒。**

我們首先來看，先將 initialValue 設定為空陣列
並在每次比較時對 acc 當中的物件做 find()
因為 find() 方法只會回傳一次 true 或 false
那麼只要沒有 id 重複的物件，就會回傳 false 並 return 一個加入 cur 的陣列
另外 [...acc, cur] 是展開運算子，用來展開 acc 陣列，用來加入 cur 到一個空的陣列中，

如果不重複，回傳一個加入cur的陣列

     // result = [
          { id: 1, name: 'Ken' },
          { id: 2, name: 'Joshua' },
          { id: 3, name: 'Shane' },
          { id: 4, name: 'Diva' }
     ]


### 結語

其實 reduce() 操作上個人覺得比較不直觀，因為這個方法的疊加器觀念比較進階，對初學者可能比較不友善，而且 reduce()　本身的運用場景是拿來做陣列值相加，如果要想更改篩選或比較當中的邏輯，可能需要先花比較多時間理解。
