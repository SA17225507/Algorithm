

> 请实现一个函数，把字符串 s 中的每个空格替换成"%20"。
> 
>  
> 
> 示例 1：
> 
> 输入：s = "We are happy."
>  输出："We%20are%20happy."

方法一：正则表达式

```javascript
var replaceSpace = function(s) {
    return s.replace(/\s/g,'%20');
};
```

方法二：
 

 - 首先判断输入是否合法，参数是字符串类型，字符串长度不能太长。  
 - 再通过split(' ')将空格隔开的单词变为字符串数组中的数组项
 - 最后通过join('%20')将各个数组项，也就是单词，连接起来完成空格的替换。

```javascript
 var replaceSpace = function(s) {
      if (typeof s == "string" && s.length >= 0 && s.length <= 10000) {
        return s.split(' ').join('%20');
      }
      return '';
};
```

方法三：编写一个函数，需要额外空间

 - 对传入的字符串进行循环
 - 当遇到不是空格的字符串时直接进行拼接
 - 当遇到是空格的字符串时，将空格转换成"%20"再进行替换
 - 最后return出替换后的字符串即可

```javascript
var replaceSpace = function (s) {
    var str = '';
    for (var i = 0; i < s.length; i++) {
        if (s[i] != " ") {
            str += s[i];
        } else {
            str += '%20';
        }
    }
    return str;
};
```
