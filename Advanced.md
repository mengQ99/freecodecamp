# 高级算法

## 1. Validate US Telephone Numbers
如果传入字符串是一个有效的美国电话号码，则返回 `true`。下面是一些有效号码的例子:

    555-555-5555
    (555)555-5555
    (555) 555-5555
    555 555 5555
    5555555555
    1 555 555 5555

区号是必须有的，如果字符串中给出了国家代码, 你必须验证其是 `1` 如果号码有效就返回 `true` ; 否则返回 `false`。
```javascript
function telephoneCheck(str) {
  
  var t = str.replace(/[()-\s+]/g, '');
  var s = str.replace(/[-\s+]/g, '');
  var re = /^\(\d{3}\)\d{7}$/g; //括号位置正确
  var re2 = /^\d{10}$/g; //无括号
  
  // 1.去掉第一位不是数字或括号的
  if(/[^\d(]/.test(str[0]))
    return false;
  
  // 2.带有国家代码的情况
  if(t.length == 11&& s[0]==1){
    return re.test(s.slice(1))||re2.test(s.slice(1));
  }else if(t.length == 10){
    // 3.不带国家代码的情况
    return re.test(s)||re2.test(s);
  }
  return false;
}

telephoneCheck("-1 (757) 622-7382"); //false
```
## 2. Symmetric Difference
创建一个函数，接受两个或多个数组，返回所给数组的 对等差分(symmetric difference) (△ or ⊕)数组。
给出两个集合 (如集合 A = {1, 2, 3} 和集合 B = {2, 3, 4}), 而数学术语 "对等差分" 的集合就是指由所有只在两个集合其中之一的元素组成的集合`(A △ B = C = {1, 4})`. 对于传入的额外集合 (如 `D = {2, 3})`, 你应该安装前面原则求前两个集合的结果与新集合的对等差分集合 `(C △ D = {1, 4} △ {2, 3} = {1, 2, 3, 4})`。
```javascript
// 1.对数组项去重
function uniq(arr){
  return arr.filter((ele, idx) => arr.indexOf(ele) == idx);
}

// 2.引用中级题目 Diff Two Arrays 的做法 返回两个数组中的差异项
function diff(arr1, arr2) {
  var a2 = arr1.filter(function(ele){
      return arr2.indexOf(ele) < 0;
  });
  var a1 = arr2.filter(function(ele){
      return arr1.indexOf(ele) < 0;
  });
  return a1.concat(a2);
}

// 3.两两迭代
function sym(args) {
  //var arr = [...arguments] ES6中类数组转换的简洁写法
  var arr = [].slice.call(arguments);
  arr = arr.map(uniq);
  return arr.reduce(diff,[]);
}
sym([1, 1, 2, 5], [2, 2, 3, 5], [3, 4, 5, 5]); //[1, 4, 5]
```

## 3. Exact Change
设计一个收银程序 `checkCashRegister()` ，其把购买价格`price`作为第一个参数 , 付款金额 `cash` 作为第二个参数, 和收银机中零钱 `cid` 作为第三个参数.

`cid` 是一个二维数组，存着当前可用的找零。当收银机中的钱不够找零时返回字符串 `"Insufficient Funds"`. 如果正好则返回字符串 `"Closed"`.
否则, 返回应找回的零钱列表,且由大到小存在二维数组中.
```javascript
function checkCashRegister(price, cash, cid) {
  cash = cash * 100;
  price = price * 100;
  
  var change = cash - price;
  var changeRest = change; 
  var result = [];
  
  if(getTotal(cid) < change) return "Insufficient Funds";
  else if(getTotal(cid) == change) return "Closed";
  
  for(var i = cid.length - 1; i >= 0 ; i--){
    var cName = cid[i][0];//钱币名称
    var cValue = fnSwitch(cName); //钱币对应金额
    var cTotal = cid[i][1] * 100; //当前类型总额
    var count = cTotal / cValue; //钱币数量
    var toReturn = 0;
    
    // 找零金额大于当前币额 且 钱箱剩余数量大于0时执行找零操作
    while(changeRest >= cValue && count > 0){
      //每循环一次 需要找零的金额减少
      changeRest -= cValue;
      count--;
      toReturn++;
    }
    
    //toReturn 记录了当前钱币类型找零的数量
    if (toReturn > 0) {
      result.push([cName, toReturn * (cValue / 100)]);
    }
    
  }

  if (getTotal(result) != change) {
    return 'Insufficient Funds';
  }
  
  return result;  
  
  //计算总额
  function getTotal(arr){
    var total = 0;
    for ( var i = 0; i < arr.length; i++ ){
      total += arr[i][1] * 100;
    } 
    return total;
  }
  
  //名称与金额的转换函数
  function fnSwitch(m) {
    switch (m) {
      case 'PENNY':
        return 1;
      case 'NICKEL':
        return 5;
      case 'DIME':
        return 10;
      case 'QUARTER':
        return 25;
      case 'ONE':
        return 100;
      case 'FIVE':
        return 500;
      case 'TEN':
        return 1000;
      case 'TWENTY':
        return 2000;
      case 'ONE HUNDRED':
        return 10000;
    }
  }
}
// Example cash-in-drawer array:
// [["PENNY", 1.01],            1美分
// ["NICKEL", 2.05],            5美分
// ["DIME", 3.10],              10美分
// ["QUARTER", 4.25],           25美分
// ["ONE", 90.00],              1美元
// ["FIVE", 55.00],             5美元
// ["TEN", 20.00],              10美元
// ["TWENTY", 60.00],           20美元
// ["ONE HUNDRED", 100.00]]     100美元

checkCashRegister(3.26, 100.00, [["PENNY", 1.01], ["NICKEL", 2.05], ["DIME", 3.10], ["QUARTER", 4.25], ["ONE", 90.00], ["FIVE", 55.00], ["TEN", 20.00], ["TWENTY", 60.00], ["ONE HUNDRED", 100.00]]);

//[["TWENTY",60],["TEN",20],["FIVE",15],["ONE",1],["QUARTER",0.5],["DIME",0.2],["PENNY",0.04]]

```
## 4. Inventory Update
依照一个存着新进货物的二维数组，更新存着现有库存(在 `arr1` 中)的二维数组。如果货物已存在则更新数量。 如果没有对应货物则把其加入到数组中，更新最新的数量。 返回当前的库存数组，且按货物名称的字母顺序排列。
```javascript
function updateInventory(arr1, arr2) {

  var flag = true;

  for(var i = 0; i < arr2.length; i++){
    //简单的循环比较 存在修改 不存在添加
    for(var j = 0; j < arr1.length; j++){
      if(arr1[j][1] == arr2[i][1]){
          arr1[j][0] += arr2[i][0];
          flag = false;
      }
    }
    if(flag) arr1.push(arr2[i]);
    flag = true;
  }
  
  return arr1.sort(by(1));
  
  //按对象属性比较函数
  function by(name){
    return function(o1, o2){
      var a = o1[name];
      var b = o2[name];
        
      if(a < b) return -1;
      if(a > b) return 1;
      return 0;
    };
  }
}

// 仓库库存示例
var curInv = [
    [21, "Bowling Ball"],
    [2, "Dirty Sock"],
    [1, "Hair Pin"],
    [5, "Microphone"]
];

var newInv = [
    [2, "Hair Pin"],
    [3, "Half-Eaten Apple"],
    [67, "Bowling Ball"],
    [7, "Toothpaste"]
];

updateInventory(curInv, newInv);

//[[88,"Bowling Ball"],[2,"Dirty Sock"],[3,"Hair Pin"],[3,"Half-Eaten Apple"],[5,"Microphone"],[7,"Toothpaste"]]
```
## 5. No repeats please
把一个字符串中的字符重新排列生成新的字符串，返回新生成的字符串里没有连续重复字符的字符串个数。连续重复只以单个字符为准。例如，aab 应该返回 2 因为它总共有6种排列 `(aab, aab, aba, aba, baa, baa)`, 但是只有两个 `(aba,aba)`没有连续重复的字符。
```javascript
function permAlone(str) {
  
  // perm函数获得全排列数组
  function perm(str){
    var res = [];
    if(str.length > 1){
      var sFirst = str[0];
      var sRest = str.slice(1);
      var s = perm(sRest);
      
      //插入字符形成新排列的过程
      for(var i = 0; i < s.length; i++){
        for(var j = 0; j < s[i].length + 1; j++){
          let t = s[i].slice(0, j) + sFirst + s[i].slice(j);
          res.push(t);
        }
      }
    //递归出口 
    }else{
      return [str];
    }
    return res;    
  }
  
  //正则检测是否含有连续字符
  var re = /^(?:(.)(?!\1))+$/, 
      count = 0,
      arr = perm(str),
      len = arr.length;
  
  for(let i = 0; i < len; i++){
    if(re.test(arr[i]))
      count++;
  }
  
  return count;

}

permAlone('aab'); // 2
```
    思路：
        将一个字符插入字符串的任意位置就可得到一个全新排列。将 1 插入至 2 的首尾两个位置得到全排列 [12, 21] 。而将 3 分别插入到每一结果项的每一位置后就得到 123 的全排列 [312, 132, 123, 321, 231, 213]。
## 6. Friendly Date Ranges
把常见的日期格式如：`YYYY-MM-DD` 转换成一种更易读的格式。易读格式应该是用月份名称代替月份数字，用序数词代替数字来表示天 `(1st 代替 1)`。
记住不要显示那些可以被推测出来的信息:         

    1. 如果一个日期区间里结束日期与开始日期相差小于一年，则结束日期就不用写年份了；在这种情况下，如果月份开始和结束日期如果在同一个月，则结束日期月份也不用写了。
    
    2. 另外, 如果开始日期年份是当前年份，且结束日期与开始日期小于一年，则开始日期的年份也不用写。
```javascript
//月份对应
var month = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];

//日期后缀
function date(n){
  var sDate;
  
  if(n < 0 || n > 31) return; 
  if(n === 1 || n === 21 || n === 31)
    sDate = n + 'st';
  else if(n === 3 || n === 23)
    sDate = n + 'rd';
  else if(n === 2 || n === 22)
    sDate = n + 'nd';
  else{
    sDate = n + 'th';
  }
  return sDate;
}


function makeFriendlyDates(arr) {
  //1. 创建日期对象
  var startDate = new Date(arr[0]);
  var endDate = new Date(arr[1]);
  
  //2. 获得起始与结束日期的年月日
  var startYear = startDate.getFullYear().toString(),
      startMonth = startDate.getMonth(),
      startDay = startDate.getDate(),
      startTime = startDate.getTime();
 
  var endYear = endDate.getFullYear().toString(),
      endMonth = endDate.getMonth(),
      endDay = endDate.getDate(),
      endTime = endDate.getTime();
  
  // 3. 获得相差天数 据此条件返回对应值
  var nowYear = new Date().getFullYear();
  var day = (endTime - startTime) / 86400000;
  var module = month[startMonth]+' '+date(startDay)+', '+startYear;
  
  if(day < 0) return undefined;
  
  //不存在两个相同的日期对象 但是毫秒数相同
  if(day === 0) return [month[startMonth]+' '+date(startDay)+', '+startYear];
  
  if(day < 31){
    if(startMonth == endMonth)
      return [month[startMonth]+' '+date(startDay), date(endDay)];
    else 
      return [month[startMonth]+' '+date(startDay), month[endMonth]+' '+date(endDay)];
  }
  
  if(startYear == nowYear) {
    return [month[startMonth]+' '+date(startDay), month[endMonth]+' '+date(endDay)];
  }  
  
  if(day < 365){
    if(startMonth == endMonth){
      return [module, month[endMonth]+' '+date(endDay)];
    }
    return [module, month[endMonth]+' '+date(endDay)];
  }
  

 return [module, month[endMonth]+' '+date(endDay)+', '+endYear];
  
}
makeFriendlyDates(["2017-01-02", "2017-01-05"]);
```
    Note: 搞这么多乱七八糟的条件实在是没有太多意义。但可以让我们复习关于时间对象的一些用法。
    
## 7. Make a Person
要求`Object.keys(bob).length == 6`，所有有参数的方法只接受一个字符串参数。
```javascript
var Person = function(firstAndLast) {
  
    var full = firstAndLast;
  
    this.getFirstName = function(){
      return full.split(' ')[0];
    };
    this.getLastName = function(){
      return full.split(' ')[1];
    };
    this.getFullName = function(){
      return full;
    };
    this.setFirstName = function(first){
      full = first +' '+ full.split(' ')[1];
    };
    this.setLastName = function(last){
      full = full.split(' ')[0] +' '+ last;
    };
    this.setFullName = function(firstAndLast){
      full = firstAndLast;
    };
};

var bob = new Person('Bob Ross');
```
## 8. Pairwise
举个例子：有一个能力数组`[7,9,11,13,15]`，按照最佳组合值为`20`来计算，只有`7+13`和`9+11`两种组合。而7在数组的索引为0，13在数组的索引为3，9在数组的索引为1，11在数组的索引为2。所以我们说函数：`pairwise([7,9,11,13,15],20)` 的返回值应该是`0+3+1+2`的和，即6。
```javascript
function pairwise(arr, arg) {
    var sum = 0, len = arr.length;
    
    //简单循环累加实现
    for(var i = 0; i < len; i++){
        //检查是否已累加 若是跳过这一项
        if(arr[i] === '') continue;
        
        for(var j = i + 1; j < len; j++){
            if(arr[j] === '') continue;
            
            if(arr[i] + arr[j] == arg){
                //将符合条件的的数组项清空
                arr[i] = arr[j] = '';
                sum += i + j;
                break;
            }
        }
    } 
    return sum;
}

pairwise([1,4,2,3,0,5], 7); //11
```
