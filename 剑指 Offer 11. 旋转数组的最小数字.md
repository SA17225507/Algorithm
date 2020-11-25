

> 把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组[3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  
> 
> 示例 1：
> 
> 输入：[3,4,5,1,2] 
> 输出：1 
> 示例 2：
> 
> 输入：[2,2,2,0,1] 
> 输出：0

方法一：我们发现数组在旋转之后，有一个分界点，在分界点的两侧数组元素仍然是递增的，而这个分界点是后一个递增数组的首元素，也就是我们要找的数组中最小的数。遍历数组，找到后一个元素比前一个元素小的位置，这后一个元素即为我们要找的那个元素。如果没有找到这样的元素，说明数组左旋了零个元素，第一个元素即为最小元素。

```javascript
var minArray = function(numbers) {
    if(numbers.length == 0) return 0;
    for(let i = 1;i<numbers.length;i++){
        if(numbers[i-1]>numbers[i]){
            return numbers[i];
        }
    }
    return numbers[0];
};
```
时间复杂度：O(n)
空间复杂度：O(1)

方法二：二分查找

 1. 两个指针left初始化为0，right初始化为n-1；mid = (left+right)/2;
 2. 当numbers[mid] > numbers[right]时，说明最小元素在右半部分，更新指针left=mid+1;
 3. 当numbers[mid] < numbers[right]时，说明右半部分的元素递增，最小元素在左半部分或者就为mid所指的元素，所以更新指针right=mid;
 4. 当numbers[mid] = numbers[right]时，我们不能判断出最小元素在哪边，但我们至少知道right所指的元素和mid所指的元素相同，所以我们可以舍弃right，更新right指针为前一位，即right--；继续判断。
 5. 直到left和right指向同一元素，该元素即为所求。

```javascript
/**
 * @param {number[]} numbers
 * @return {number}
 */
var minArray = function(numbers) {
    if(numbers.length == 0) return 0;
    let n = numbers.length;
    //如果第一个数小于最后一个数，说明左旋了0个元素，第一个元素即为最小
    if(numbers[0] < numbers[n-1]) return numbers[0];

    let left = 0,right = n-1;
    while(left<right){
        let mid = (left+right)>>1;
        if(numbers[mid] > numbers[right]){
            left = mid+1;
        }else if(numbers[mid] < numbers[right]){
            right = mid;
        }else{
            right--;
        }
    }
    return numbers[left];
};
```
复杂度分析

 * 时间复杂度：平均时间复杂度为O(logn)，其中 n是数组numbers的长度。如果数组是随机生成的，那么数组中包含相同元素的概率很低，在二分查找的过程中，大部分情况都会忽略一半的区间。而在最坏情况下，如果数组中的元素完全相同，那么while 循环就需要执行 n 次，每次忽略区间的右端点，时间复杂度为 O(n)。 
 * 空间复杂度：O(1)。
