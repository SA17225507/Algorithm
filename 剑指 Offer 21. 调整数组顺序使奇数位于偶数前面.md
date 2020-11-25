
> 输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。
> 
>  
> 
> 示例：
> 
> 输入：nums = [1,2,3,4] 
> 输出：[1,3,2,4]  
> 注：[3,1,2,4] 也是正确的答案之一。

方法一：
第一次循环依次找到偶数和奇数，并且将其分别存放到新开辟的空间中。
第二次循环将存放偶数和奇数的空间“连接”在一起。

```javascript
var exchange = function(nums) {
    const arr = []; // 奇数数组
    const brr = []; // 偶数数组
    nums.forEach(item => {
        item & 1 ? arr.push(item) : brr.push(item);
    });

    return arr.concat(brr);
};
```
没有改变原数组，没有改变奇数之间以及偶数之间的相对顺序。
时间复杂度 O(N)
空间复杂度 O(N)

方法二：
第一次遍历记录奇数的个数，即为偶数的开始索引。
第二次遍历根据奇数偶数改变奇数偶数的索引放入相应的位置。

```javascript
var exchange = function(nums) {
    let oddBegin = 0,oddCount = 0;
    for(let i = 0;i<nums.length;i++){
        if(nums[i] & 1) oddCount++; 
    }
    let res = [];
    for(let i = 0;i<nums.length;i++){
        if(nums[i] & 1){
            res[oddBegin++] = nums[i];
        }else{
            res[oddCount++] = nums[i];
        }
    }
    return res;
};
```
没有改变原数组，没有改变奇数之间以及偶数之间的相对顺序。
时间复杂度 O(N)
空间复杂度 O(N)

方法三: 双指针交换

 - 双指针是分别指向数组头部的指针 i，与指向数组尾部的指针 j。
 - i 向右移动，直到遇到偶数；j 向左移动，直到遇到奇数 
 - 检查 i 是否小于 j，若小于，交换 i 和 j指向的元素，回到上一步骤继续移动；否则结束循环 

```javascript
var exchange = function(nums) {
    const length = nums.length;
    if (!length) {
        return [];
    }

    let i = 0,
        j = length - 1;
    while (i < j) {
        while (i < length && nums[i] & 1) i++;
        while (j >= 0 && !(nums[j] & 1)) j--;

        if (i < j) {
            [nums[i], nums[j]] = [nums[j], nums[i]];
            i++;
            j--;
        }
    }
    return nums;
};
```
改变原数组，改变奇数之间以及偶数之间的相对顺序。
时间复杂度是 O(N)
空间复杂度是 O(1)
