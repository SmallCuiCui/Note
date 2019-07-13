### ES6

#### map

遍历数组的每一项，函数参数作为数组里的每一项，在return里操作item或里面的内容，return返回一个新数组。

```javascript
例1：
var data = [1, 2, 3, 4];

var arrayOfSquares = data.map(function (item) {
  return item * item;
});

alert(arrayOfSquares); // 1, 4, 9, 16
```

```javascript
var users = [
  {name: "张含韵", "email": "zhang@email.com"},
  {name: "江一燕",   "email": "jiang@email.com"},
  {name: "李小璐",  "email": "li@email.com"}
];

var emails = users.map(function (user) { return user.email; });

console.log(emails.join(", ")); // zhang@email.com, jiang@email.com, li@email.com
```

#### filter

过滤数组中不满足条件的元素

```javascript
例1：
var newarr = [
  { num: 1, val: 'ceshi', flag: 'aa' },
  { num: 2, val: 'ceshi2', flag: 'aa2'  }
]
console.log(newarr.filter(item => item.num===2 ))
//[{ num: 2, val: 'ceshi2', flag: 'aa2'  }]
```

```javascript
例2(去重):
var arr = [1, 2, 2, 3, 4, 5, 5, 6, 7, 7,8,8,0,8,6,3,4,56,2];
var arr2 = arr.filter((x, index,self)=>self.indexOf(x)===index)  
console.log(arr2); //[1, 2, 3, 4, 5, 6, 7, 8, 0, 56]
```

#### reduce

遍历数组里的每一项，上一项与下一项进行操作。

```javascript
var total = [ 0, 1, 2, 3 ].reduce(( acc, cur ) => {
    return acc + cur
}, 0);
console.log(total)   // 6
```

**acc为上一项的结果，cur为下一项，函数的后面可以写一个参数代表初始值，可用来去重**

#### forEach

遍历数组的每一项，可用来循环绑定事件

```javascript
var myArray=[
        {id:1,name:"a"},
        {id:2,name:"b"},
        {id:3,name:"c"},
        {id:4,name:"d"},
        ]
myArray.forEach((item,index)=>{
    console.log(item.id);
        //1,2,3,4,5
})
```

