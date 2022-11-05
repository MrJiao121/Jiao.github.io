# 扩展运算符
 可以平铺数组或者对象

- 扩展运算是一种浅拷贝的方式
-
 ```js
let [name,...arr]=[1,2,4]
//解构只能放在最后一位
```
- 将字符串转化为数组 ```js [...'name']=>['n','a','m','e'] ```
- 伪数组也可以使用扩展运算符（部署 Iterator ）？
 替代函数apply方法
 apply()方法传入的是数组

 ## apply
 apply 方法会改变当前函数`this`的指向
 ```js

 let obj  = {
  name:'duoduo',
  sex:'f'
}
function Person(){
  this.name='11',
  this.age=12;
  console.log(this.name); //11
}
Person.apply(obj);

console.log(obj) // {name: '11', sex: 'f', age: 12, fun: ƒ}

console.log(obj.name);//11
console.log(obj.sex);//f
 ```
 `Person.apply`方法中，将当前this即Person方法中的this和obj合并
 未执行Person方法时

 obj={
  fun:function xxx(){},
  name:'duoduo',
  sex:'f'

 }
 执行Person方法后  会覆盖obj的同名属性

  obj = {
  fun:function xxx(){},
  name:'11',
  sex:'f',
  age:12

 }

 最终执行完Person.apply(obj)后 ， obj.name已经被覆盖为Person内的值了

 ### 实现apply方法
 ```js
  Function.prototype.myApply=function(){
    let object = arguments[0]!=void 0 ? arguments[0] : window;
    let arg = arguments[1]||[];
    object.func= this;//this 指向调用的函数
   let result =  object.func(...arg);
   return result;
 }
 ```
## Array.from()
- 将伪数组转化为真实的数组
- 必须有length属性
- 将函数arguments转化为真实的数组
- 只要是部署了 Iterator 接口的数据结构，Array.from()都能将其转为数组。
- 第二个参数可以对每个元素进行处理

```js

let arraylike = {
  0:'a',
  1:'b',
  2:'c',
  length: 3
}
Array.from(arraylike) //['a', 'b', 'c']
Array.from(arraylike,(x)=>x+'A'); // ['aA', 'bA', 'cA']
Array.from({length:2}); // [undefined, undefined]
```
## Array.of() 
- 将一组值转化为数组
- 返回参数组成的数组，如果没有 就返回空数组
 ```js
Array.of(1,2,3) //[1,2,3]

Array() //[]
Array(2)//[,,]
Array.of(2) //[2]
 ```
 自实现`Array.of`
 ```js
 Array.prototype.ArrayOf=function(){
  return [].slice.apply(arguments);
 }
 ```
 ## copyWithIn
 将指定成员位置的元素复制到其他位置  会改变原数组
 ```js
 
Array.prototype.copyWithIn = function(target,start,end){
  /* 
  target : 开始替换的位置
  start （可选） 从该位置读取元素 默认为0
  end （可选） 读取截止到该位置  默认等于数组长度 
  */

}
[1,2,3,4,5].copyWithIn(0,3)
// 读取4,5  [4,5,3,4,5]
// 将3号位复制到0号位
[1, 2, 3, 4, 5].copyWithin(0, 3, 4)

// [4, 2, 3, 4, 5]

// -2相当于3号位，-1相当于4号位
[1, 2, 3, 4, 5].copyWithin(0, -2, -1)

// [4, 2, 3, 4, 5]

// 将3号位复制到0号位
[].copyWithin.call({length: 5, 3: 1}, 0, 3)
// {0: 1, 3: 1, length: 5}

// 将2号位到数组结束，复制到0号位
let i32a = new Int32Array([1, 2, 3, 4, 5]);
i32a.copyWithin(0, 2);
// Int32Array [3, 4, 5, 4, 5]

// 对于没有部署 TypedArray 的 copyWithin 方法的平台
// 需要采用下面的写法
[].copyWithin.call(new Int32Array([1, 2, 3, 4, 5]), 0, 3, 4);
// Int32Array [4, 2, 3, 4, 5]


 ```
##  find findIndex  findLast  findLastIndex 
find 找出第一个符合要求的元素
find 回调函数可接受三个参数
find 第二个参数 回调函数this的指向 
```js
[1,2,3].find((value,index,arr)=>{})
[11,22,33].find(function(v){return this.age>v},{age:12})
```
findIndex 返回第一个符合元素的下标
findLast 找出最后一个符合元素
findLastIndex 返回最后一个符合元素的下标

## fill()
填充数组
fill(num,start,end)
num 填充的数组
start 从那个位置开始填充
end 结束填充
```js
let arr = new Array(3).fill([])
arr[0].push(1)
//[[1], [1], [1]]
```
## entries() keys() values()
entries() 对键值对的遍历
可以通过for...of遍历
## includes 
可以判断NaN


