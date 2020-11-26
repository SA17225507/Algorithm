

> 从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为0 ，可以看成任意数字。A 不能视为 14。
> 
>  
> 
> 示例 1:
> 
> 输入: [1,2,3,4,5] 
> 输出: True  
> 
> 示例 2:
> 
> 输入: [0,0,1,2,5] 
> 输出: True
> 限制：
> 数组长度为 5 
> 数组的数取值为 [0, 13] .

思路：
1，除0以外，最大值最小值之差不能超过5
2，除0以外，5个数中不能有重复数字

```javascript
var isStraight = function(nums) {
    if(nums.length !== 5) return false;
    nums.sort((a,b) => a-b);
    let min = 14,max = 0;
    for(let i = 0;i<5;i++){
        if(nums[i] == 0) continue;
        if(nums[i] != nums[i+1]){
            max = Math.max(max,nums[i]);
            min = Math.min(min,nums[i]);
        }else{
            return false;
        }
    }
    if(max - min >=5) return false;
    return true;
};
```

用位运算来判断是否重复，数字范围为1-13，每一个bit对应一个数字，出现过，这个bit位为1，否则为0

```javascript
var isStraight = function(nums) {
    if(nums.length !== 5) return false;
    let max = 0,min = 14,flag = 0;
    for(let i = 0;i< 5;i++){//除0以外，5个数中不能有重复数字
        if(nums[i] < 0 || nums[i] > 13) return false;//数字不合法
        if(nums[i] == 0) continue;//为0可以为任意数字，直接跳过
        //位运算判断数字是否重复
        //将1左移nums[i]位，相当于把第nums[i]位置为1.然后与flag与运算
        //如果大于0，说明flag的第nums[i]位已经为1，数字重复来，返回false。
        if(((1<<nums[i]) & flag) > 0) return false;
        //该位不为1，说明数字第一次出现，按位或将flag的该位置填充为1。
        flag = flag | (1 << nums[i]);
        //更新最值
        if(max < nums[i]) max = nums[i];
        if(min > nums[i]) min = nums[i];
    }
    //除0以外，最大值最小值之差不能超过5
    if(max - min >= 5) return false;
    return true;
};
```
