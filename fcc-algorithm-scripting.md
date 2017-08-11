# Freecodecamp Algorithm Scripting

标签（空格分隔）： 未分类

---

## 1. 初级算法
### 1. Reverse a String 翻转字符串
```javascript
function reverseString(str) {
  return str.split('').reverse().join('');
}
reverseString("hello");//"olleh"
```
通过字符串和数组之间的转换，借用数组内置的`reverse`方法，实现字符串类型的翻转。由此可以写出字符串类型的拓展`reverse()`。
```javascript
if(!String.prototype.reverse){
    String.prototype.reverse = function(){
        return this.split('').reverse().join('');
    };
}
```
    注意：
        1. split()不传参情况下将返回一个仅有一项，内容是整个字符串的数组。['hello']
        2. join()默认分隔符为逗号。 
### 2. Check for Palindromes 检查回文数
如果一个字符串忽略标点符号、大小写和空格，正着读和反着读一模一样，那么这个字符串就是palindrome(回文)。
```javascript
function palindrome(str) {
  str = str.replace(/[^a-zA-Z0-9]/g,'').toLowerCase();
  return str === str.reverse();
}
palindrome("race car");// true
```
### 3. Find the Longest Word in a String 找出最长单词
在句子中找出最长的单词，并返回它的长度。函数的返回值应该是一个数字。
```javascript
function findLongestWord(str) {
  var aLen = [];//数组保存每个单词的长度
  var arr = str.split(' ');
  for(var i = 0;i < arr.length;i++ ){
    aLen.push(arr[i].length);
  }
  return Math.max.apply(null,aLen);
}
findLongestWord("The quick brown fox jumped over the lazy dog");//6
```
### 4. Title Case a Sentence 首字母大写
 - 简单粗暴的方法。
```javascript
function titleCase(str){
    var arr = str.toLowerCase().split(' ');
    for(var i = 0; i < arr.length; i++){
        arr[i] = arr[i][0].toUpperCase() + arr[i].slice(1);
    }
    return arr.join(' ');
}
```
- 正则表达式最简洁。
```javascript
function titleCase(str) {
    //   将下式正则替换为 /^\S/g ,实现仅将句首字母大写
    return str.toLowerCase().replace(/(\s|^)[a-z]/g,function(s){return s.toUpperCase();});
}
```
    注意：
        1. 字符串只读，不可改变原字符串。
        2. text-transform: capitalize;
        3. \b匹配单词边界，[\b]匹配一个空格。
### 5. Chunky Monkey 分割数组
把一个数组`arr`按照指定的数组大小`size`分割成若干个数组块。主要考察`slice`的用法。
    
    例如:
        chunk([1,2,3,4],2)=[[1,2],[3,4]];
        chunk([1,2,3,4,5],2)=[[1,2],[3,4],[5]];
```javascript
function chunk(arr, size) {
    var len = Math.ceil(arr.length/size);
    var aResult = [];
    for(var i = 0; i < len; i++){
        aResult.push(arr.slice(i*size, (i + 1)*size));
    }
    return aResult;
}
```
### 6. Mutations 比较字符串
如果数组第一个字符串元素包含了第二个字符串元素的所有字符，函数返回`true`。
```javascript
function mutation(arr) {
    var str = arr[0].toLowerCase();
    var subStr = arr[1].toLowerCase();
    for(var i = 0;i < subStr.length;i++){
        if(!~str.indexOf(subStr[i])){ //str.indexOf(subStr[i])==-1 可读性更好
            return false;
        }
    }
    return true;
}
mutation(["hello", "hey"]); //false
mutation(["hello", "heo"]); //true
```
    注意：不要因为追求代码的简洁美观而舍弃可读性。
### 7. Seek and Destroy 摧毁数组
实现一个摧毁`(destroyer)`函数，第一个参数是待摧毁的数组，其余的参数是待摧毁的值。函数返回摧毁数组后剩下的数组。
```javascript
function destroyer(arr) {
    var arg=Array.prototype.slice.call(arguments).slice(1);
    return arr.filter(function(ele){
        return arg.indexOf(ele) == -1;
    });  
}
destroyer([1, 2, 3, 1, 2, 3], 2, 3);//[1, 1]
destroyer(["tree", "hamburger", 53], "tree", 53);//["humburger"]
```
    注意：Array.prototype.slice.call(); 实现类数组转换为数组。
### 8. Caesars Cipher 凯撒密码
写一个`ROT13`函数，实现输入加密字符串，输出解密字符串。字母会移位13个位置。由`'A' ↔ 'N'`, `'B' ↔ 'O'`，以此类推。
所有的字母都是大写，不要转化任何非字母形式的字符(例如：空格，标点符号)，遇到这些特殊字符，跳过它们。
```javascript
function rot13(str) {
    var arr=[];
    for(var i=0;i<str.length;i++){  
        if(str.charCodeAt(i)<65||str.charCodeAt(i)>90){  
            arr.push(str.charAt(i));  
        }else if(str.charCodeAt(i)>77){
            arr.push(String.fromCharCode(str.charCodeAt(i)-13));  
        }else{  
            arr.push(String.fromCharCode(str.charCodeAt(i)+13));  
        }  
   }  
   return arr.join("");  
}
rot13("SERR PBQR PNZC");//FREE CODE CAMP
```
    注意：
        1. str.charCodeAt(index)返回给定索引处字符的Unicode编码值。
        2. 静态String.fromCharCode(num1,...numN)方法返回使用指定的Unicode值序列创建的字符串。
## 2.中级算法