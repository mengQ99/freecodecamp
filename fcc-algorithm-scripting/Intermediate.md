# 中级算法


### 1. Sum All Numbers in a Range
传递给你一个包含两个数字的数组。返回这两个数字和它们之间所有数字的和。最小的数字并非总在最前面。
```javascript
function sumAll(arr) {
    // 1.取得数组中的最大最小值
    var min = Math.min.apply(null, arr);
    var max = Math.max.apply(null, arr);
    var len = max-min-1;
    // 2.将数组补充完整
    for(var i = 0; i < len; i++){
        arr.push(++min);
    }
    // 3.迭代累加求和
    return arr.reduce(function(sum,value){
        return sum+value;
  });
}
sumAll([4, 1]);//10
```
### 2. Diff Two Arrays
比较两个数组，然后返回一个新数组，该数组的元素为两个给定数组中所有独有的数组元素。换言之，返回两个数组的差异。
```javascript
function diff(arr1, arr2) {
    // 1. 分别求出自身数组的独有值
    var a2 = arr1.filter(function(ele){
        return arr2.indexOf(ele) < 0;
    });
    var a1 = arr2.filter(function(ele){
        return arr1.indexOf(ele) < 0;
    });
    // 2. 两结果数组合并
    return a1.concat(a2);
}
diff([1, "calf", 3, "piglet"], [1, "calf", 3, 4]);//[4, "piglet"]
```
### 3. Roman Numeral Converter
将给定的数字转换成罗马数字。所有返回的 [罗马数字][1] 都应该是大写形式。
```javascript
function convert(num) {
  var aNum = [1000,900,500,400,100,90,50,40,10,9,5,4,1];
  var aRoman = ['M','CM','D','CD','C','XC','L','XL','X','IX','V','IV','I'];
  var str = '';
  aNum.forEach(function(ele, index){
    while(num >= ele){
      str += aRoman[index];
      num -= ele;
    }
  });
  return str;
}
convert(36); //"XXXVI"
```
### 4. Where art thou
写一个 `function`，它遍历一个对象数组（第一个参数）并返回一个包含相匹配的属性-值对（第二个参数）的所有对象的数组。如果返回的数组中包含 `source` 对象的属性-值对，那么此对象的每一个属性-值对都必须存在于 `collection` 的对象中。

例如，如果第一个参数是 [{ first: "Romeo", last: "Montague" }, { first: "Mercutio", last: null }, { first: "Tybalt", last: "Capulet" }]，第二个参数是 { last: "Capulet" }，那么你必须从数组（第一个参数）返回其中的第三个对象，因为它包含了作为第二个参数传递的属性-值对。

```javascript
function where(collection, source) {
  var s = Object.keys(source);
  var f = 0,arr=[];
  for(var i = 0; i < collection.length; i++){
    f = 0;
    for(var j = 0;j < s.length; j++){
      // 条件：有自身属性且属性值相同
      if(collection[i].hasOwnProperty(s[j])&&(collection[i][s[j]]==source[s[j]])){
        f++;
      }
    }
    if(f == s.length) 
        arr.push(collection[i]);
  }
  return arr;
}
where([{ first: "Romeo", last: "Montague" }, { first: "Mercutio", last: null }, { first: "Tybalt", last: "Capulet" }], { last: "Capulet" });// [{"first":"Tybalt","last":"Capulet"}]
```
### 5. Pig Latin
`Pig Latin` 把一个英文单词的第一个辅音或辅音丛（consonant cluster）移到词尾，然后加上后缀 `"ay"`。如果单词以元音开始，你只需要在词尾添加 `"way"` 就可以了。
```javascript
function translate(str){
  var s = str.replace(/[aeiou]/g,'|');//"c|ns|n|nt"
  var idx = s.indexOf('|');
  var last = str.slice(idx);
  //不要这样写: 性能不会提高且可读性极差 简单的条件句就可以
  return ~idx?idx?last+str.slice(0,idx)+'ay':last+'way':'error';
}
translate("consonant");//"onsonantcay"
```
    这个题目刚开始是真不会做，不知道辅音从到底如何划分。实现起来很简单，只是欠缺常识，只要以元音为分隔符再利用正则将辅音揪出来再做其他操作就可以了。
### 6. Missing letters 
从传递进来的字母序列中找到缺失的字母并返回它。如果所有字母都在序列中，返回 `undefined`。
```javascript
function fearNotLetter(str) {
  var arr = str.split('');
  var result = arr.filter(function(ele, idx, arr){
    if(idx! == 0){
      return ele.charCodeAt(0) - arr[idx-1].charCodeAt(0) > 1;
    }
  });
  if(result.toString()){
    return String.fromCharCode(result[0].charCodeAt(0)-1);
  }
  return undefined;
}
fearNotLetter("abc");
```
    简单思路：找出两值之差大于1的数即可，单用一个for循环就可以啦。
### 7. Sorted Union
传入两个或两个以上的数组，返回一个以给定的原始数组排序的不包含重复值的新数组。

```javascript
function unite(arr1, arr2, arr3) {
  //es6新方法 类数组转为数组
  var args = Array.from(arguments);
  var arr = args.reduce(function(prev, cur){
    return prev.concat(cur);
  });
  //利用filter()数组去重
  return arr.filter(function(ele, idx, array){
    return array.indexOf(ele) === idx;  
  });
}
unite([1, 3, 2], [5, 2, 1, 4], [2, 1]);//[1, 3, 2, 5, 4]
```
    注意：这里用巧妙运用reduce()实现了二维数组的合并。
#### **数组去重**
```javascript
// 1. 基本方法 新建数组遍历添加
function uniqueArr(arr){
  var newArr = [];
  for(var i = 0; i < arr.length; i++){
    if(newArr.indexOf(arr[i]) == -1)
      newArr.push(arr[i]);
    }
    return newArr;
}

// 2. 数组排序后去重 但这种方法改变了原数组的顺序
function uniqueArr(arr){
  var newArr = [];
  arr = arr.sort(function(a, b){ return a - b; });
  for(var i = 0; i < arr.length; i++){
    if(arr[i] !== arr[i+1]||i == arr.length - 1)
      newArr.push(arr[i]);
  }
  return newArr;
}

// 3. 利用对象属性的存在性
function uniqueArr(arr){
  var obj = {};
  var newArr = [];
  for(var i = 0; i < arr.length; i++){
    if(!obj[arr[i]]){
      newArr.push(arr[i]);
      obj[arr[i]] = 1;
    }
  }
  return newArr;
}

// 4. 上面说过的迭代方法去重 比较简洁
function unique(arr){
  return arr.filter(function(ele, idx){
    return arr.indexOf(ele) === idx;  
  });
}

// 5. 利用ES6: Set/Map 数据结构去重
function unique(arr){
  //Array.form可将set结构转为数组
  return Array.from(new Set(arr));
}

```
### 8. Convert HTML Entities
将字符串中的字符 `&`、`<`、`>`、`"` （双引号）, 以及 `'` （单引号）转换为它们对应的 `HTML` 实体。
```javascript
function convert(str) {
  var obj = {
    '<': '&lt;',
    '>': '&gt;',
    '&': '&amp;',
    '"': '&quot;',
    "'": '&apos;'
  };
  return str.replace(/[<>&"']/g,function(a){
    return obj[a];
  });
}
convert("Dolce & Gabbana");//Dolce &​amp; Gabbana
```
    这个方法在《js语言精粹》一书中提到过，这种对象的用法通常优于switch语句。
### 9. Sum All Odd Fibonacci Numbers 
给一个正整数`num`，返回小于或等于`num`的斐波纳契奇数之和。斐波纳契数列中的前几个数字是 1、1、2、3、5 和 8，随后的每一个数字都是前
两个数字之和。
例如，`sumFibs(4)`应该返回 5，因为斐波纳契数列中所有小于4的奇数是 1、1、3。
    
    提示：此题不能用普通递归来实现斐波纳契数列。因为当num较大时，内存会溢出，推荐用数组来实现。
```javascript
function sumFibs(n) {
  // 1.数组存储数列 
  var arr = [1,1];
  for(var i = 2;i <= n; i++){
    arr[i] = arr[i-2] + arr[i-1];
  }
  // 2.按照题目条件过滤数组 
  arr = arr.filter(function(e){
    return e%2!==0 && e<=n;
  });
  // 3.数组累加求和得出结果
  return arr.reduce(function(prev, cur){
    return prev+cur;
  });
}
sumFibs(1000); //1785
```
#### **斐波那契数列**
```javascript
// 1.递归实现 占内存大 因为包含很多重复计算
function Fib(n){
  return n > 2 ? Fib(n-2) + Fib(n-1) : 1;
}

// 2.数组实现 性能优于递归
function Fib(n){
  var arr = [1,1];
  for(var i = 2;i <= n; i++){
    arr[i] = arr[i-2] + arr[i-1];
  }
}

// 3.简单循环运算实现 性能最优
function Fib(n){
  var a = 1, b = 1;
  for(var i = 2; i < n; i++){
    b = a + b;
    a = b - a;
  }
}

// 4. 尾递归 与迭代类似
function Fib(n, t1, t2){
  if(n == 0) return t1;
  return Fib(n-1, t2, t1+t2);
}
```
### 10. Sum All Primes
求小于等于给定数值的质数之和。只有 1 和它本身两个约数的数叫质数。例如，2 是质数，因为它只能被 1 和 2 整除。1 不是质数，因为它只能被自身整除。
```javascript
function sumPrimes(num) {
  var res = 2;
  var flag = 0;
  // 1.循环所有小于等于num的数
  for(var i = 3; i<= num; i++){
    flag = 0;
    // 2.判断当前数是否为质数
    for(var j = 2; j < i; j++){
      if( i%j === 0 ){
        flag = 1;
        break;
      }
    }
    // 3.将质数累加
    if(!flag)
      res += i;
  }
  return res;
}
sumPrimes(10);// 17
```
### 11. Smallest Common Multiple
找出能被两个给定参数和它们之间的连续数字整除的最小公倍数。范围是两个数字构成的数组，两个数字不一定按数字顺序排序。
```javascript
function smallestCommons(arr) {
  var t, d, min;
  // 1.找出最大、小值补全数组
  if(arr[0]>arr[1]){
    t = arr[0]; 
    arr[0] = arr[1]; 
    arr[1] = t;
  }
  d = arr[1]-arr[0];
  min = arr[0];
  for(var i=0;i < d-1; i++){
    arr.push(++min);
  }
  // 2.两两迭代求公倍数
  return arr.reduce((prev, cur) => lcm(prev, cur), arr[0]);
}

//求最小公倍数: 两数的最小公倍数 * 最大公约数 = 两数之积
function lcm(a, b){
  var t, m, x = a * b;
  if(a < b){
    t = a;
    a = b;
    b = t;
  }
  while(b!==0){
    a = a > b ? a : b;
    m = a % b;
    a = b;
    b = m;
  }
  return x/a;
}

smallestCommons([1,5]);// 60
```
### 12. Steamroller
对嵌套的数组进行扁平化处理。你必须考虑到不同层级的嵌套。
#### **数组扁平化**
```Javascript
// 1. 普通递归实现
function flat(arr) {
    var result = [];
    for (var i = 0, len = arr.length; i < len; i++) {
        if (Array.isArray(arr[i])) {
            result = result.concat(flat(arr[i]))
        }else {
            result.push(arr[i])
        }
    }
    return result;
}

// 2. reduce迭代方法
function flat(arr) {
    return arr.reduce(function(prev, cur){
        return prev.concat(Array.isArray(cur) ? flat(cur) : cur)
    }, [])
}

// 3. 扩展运算符(...arr)
function flat(arr){
    while( arr.some((item) => Array.isArray(item)) ){
        arr = [].concat(...arr);
    }
    return arr;
}

// 4. 另一种方法是用ES6中的Generator函数实现。
function flat(arr) {
    var res = [];
    for(var i of flat(arr)){
        res.push(i);
    }
    return res;
}
// yield* 遍历方法
var flat = function* (arr){
    var len=arr.length;
    for(var i = 0; i < len; i++){
        var item = arr[i];
        if(Array.isArray(item)){
            yield* flat(item);
        }else{
            yield item;
        }
    } 
};
```
### 13. Arguments Optional
创建一个计算两个参数之和的参数。如果只有一个参数，则返回一个 函数，该请求一个参数然后返回求和的结果。
例如，`add(2, 3)` 应该返回 `5`，而 `add(2)` 应该返回一个 `function`。如果两个参数都不是有效的数字，则返回 `undefined`。
```javascript
function add(a, b){
  
  if(typeof a == 'number' && typeof b == 'number' ) 
    return a+b;
  
  if(!b && typeof a == 'number'){
    return function(c){
      if(typeof c == 'number')
        return a+c;
    };    
  }  
}

add(2)(3); // 5
add(2, 3); // 5
```
    简单的判断条件返回闭包函数求和。附上柯里化函数实现。
```javascript
function curry(fn){
  var args1 = [].slice.call(arguments, 1);
  return function(){
    var args2 = [].slice.call(arguments);
    var args = args1.concat(args2);
    return fn.apply(null, args);
  }
}
```

    中级算法的部分题目结束。

  [1]: http://www.mathsisfun.com/roman-numerals.html
  
  
  
