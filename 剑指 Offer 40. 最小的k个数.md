

> 输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。
> 
>  
> 
> 示例 1：
> 
> 输入：arr = [3,2,1], k = 2 
> 输出：[1,2] 或者 [2,1] 
> 示例 2：
> 
> 输入：arr = [0,1,2,1], k = 1 
> 输出：[0]  
> 
> 限制：
> 0 <= k <= arr.length <= 10000 
> 0 <= arr[i] <= 10000

方法一：排序

```javascript
var getLeastNumbers = function(arr, k) {
    if(arr.length == 0 || k > arr.length || k < 1) return [];
    return arr.sort((a,b) => a-b).slice(0,k);
};
```
改变原数组,使用高级排序（代码用的是快排）
复杂度分析
时间复杂度：O(nlogn)，其中 n是数组 arr 的长度。算法的时间复杂度即排序的时间复杂度。
空间复杂度：O(logn)，排序所需额外的空间复杂度为O(logn)。


方法二：快排思想Partition方法
我们可以借鉴快速排序的思想。我们知道快排的划分函数每次执行完后都能将数组分成两个部分，小于等于分界值index 的元素的都会被放到数组的左边，大于的都会被放到数组的右边，然后返回分界值的下标。与快速排序不同的是，快速排序会根据分界值的下标递归处理划分的两侧，而这里我们只处理划分的一边。（二分）index为k，比k小的放前面，比k大的放后面，输出前k个数。
回顾快速排序中的 partition 操作，可以将元素arr[0]放入排序后的正确位置，并且返回这个位置index。利用 partition 的特点，算法流程如下：
如果index = k-1，（下标）说明第 k 个元素已经放入正确位置，返回前 k 个元素
如果k-1 < index，第 k 个元素在[left, index - 1]之间，缩小查找范围，继续查找
如果index < k-1，第 k 个元素在[index + 1, right] 之间，缩小查找范围，继续查找
```javascript
/**
 * @param {number[]} arr
 * @param {number} k
 * @return {number[]}
 */
var getLeastNumbers = function(arr, k) {
    const length = arr.length;
    if (k >= length) return arr;
    let left = 0,
        right = length - 1;
    let index = partition(arr, left, right);
    while (index !== k-1) {
        if (index < k-1) {
            left = index + 1;
            index = partition(arr, left, right);
        } else if (index > k-1) {
            right = index - 1;
            index = partition(arr, left, right);
        }
    }

    return arr.slice(0, k);
};
function partition(arr, start, end) {
    const k = arr[start];
    let left = start + 1,
        right = end;
    while (1) {
        while (left <= end && arr[left] <= k) ++left;
        while (right >= start + 1 && arr[right] >= k) --right;

        if (left >= right) {
            break;
        }

        [arr[left], arr[right]] = [arr[right], arr[left]];
        ++left;
        --right;
    }
    [arr[right], arr[start]] = [arr[start], arr[right]];
    return right;
}
```

```javascript
var getLeastNumbers = function(arr, k) {
    const len = arr.length;
    if(!len) {
        return [];
    }

    const qucikSelect = (nums, start, end) => {
        if(start >= end) {
            return;
        }
        const pivot = nums[start];
        let i = start, j = end;

        while(i < j) {
            // 找出右边第一个小于参照数的下标 j 并记录
            while(i < j && nums[j] >= pivot) {
                j--;
            }
            if(i < j) {
                nums[i++] = nums[j];
            }
            // 找出左边第一个大于参照数的下标 i ，并记录
            while(i < j && nums[i] < pivot) {
                i++;
            }
            if(i < j) {
                nums[j--] = nums[i];
            }
        }
        nums[i] = pivot;

        if(i === k - 1) {
            return;
        } else if(i < k - 1) {
            qucikSelect(nums, i + 1, end);
        } else {
            qucikSelect(nums, start, i - 1);
        }
    }

    qucikSelect(arr, 0, len - 1);

    return arr.slice(0, k);
};
```

```javascript
/**
 * @param {number[]} arr
 * @param {number} k
 * @return {number[]}
 */
var getLeastNumbers = function(arr, k) {
    if(arr.length == 0 || k > arr.length || k < 1) return [];
    
    let left = 0,
        right = arr.length - 1;
    let index = Partition(arr,left,right);
    while(index !== k-1){
         if (index < k-1) {
            left = index + 1;
            index = Partition(arr,left,right);
        } else if (index > k-1) {
            right = index - 1;
            index = Partition(arr,left,right);
        }
    }
    return arr.slice(0,k);
};

function Partition(arr,l,r){
    let key = arr[l];
    while(l<r){
        while(l<r && arr[r] >= key) r--;
        [arr[l],arr[r]] = [arr[r],arr[l]];
        while(l<r && arr[l] <= key) l++;
        [arr[l],arr[r]] = [arr[r],arr[l]];
    }
    return r;
}
```

改变原数组
复杂度分析

 - 时间复杂度：期望为O(n) ，最坏情况下的时间复杂度为 O(n^2)。情况最差时，每次的划分点都是最大值或最小值，一共需要划分 n -1次，而一次划分需要线性的时间复杂度，所以最坏情况下时间复杂度为 O(n^2)。
 - 空间复杂度：期望为O(logn)，递归调用的期望深度为O(logn)，每层需要的空间为O(1)，只有常数个变量。最坏情况下的空间复杂度为O(n)。最坏情况下需要划分 n次，函数递归调用最深n−1 层，而每层由于需要O(1)的空间，所以一共需要O(n) 的空间复杂度。

方法三：最大堆
堆是一种非常常用的数据结构。最大堆的性质是：节点值大于子节点的值，堆顶元素是最大元素。利用这个性质，整体的算法流程如下：

 - 从数组中取前 k 个数（ 0 到 k-1 位），构造一个大顶堆。
 - 从下标 k 继续开始依次遍历数组的剩余元素，每一个数据都和大顶堆的堆顶元素进行比较。
 - 如果元素小于堆顶元素，则将这个元素替换掉堆顶元素，调整成新的最大堆。
 - 如果元素大于/等于堆顶元素，不做操作，继续遍历下一元素。
 - 遍历完成后，堆中的数据就是前 K 小的数据。


```javascript
/**
 * @param {number[]} arr
 * @param {number} k
 * @return {number[]}
 */
var getLeastNumbers = function(arr, k) {
    if(arr.length == 0 || k<1 || k>arr.length) return [];
    let heap = arr.slice(0,k);//先将前k个数放入数组
    buildHeap(heap);//建堆
    for(let i = k;i<arr.length;i++){//遍历剩下的数字
        if(arr[i] < heap[0]){//比堆顶小，替换堆顶然后调整为最大堆
            heap[0] = arr[i];
            maxHeap(heap,0);
        }
    }
    return heap;
};
function buildHeap(arr,i){
    //从第一个非叶子节点向上调整
    for(let i = (arr.length>>1)- 1;i>=0;i--){
        maxHeap(arr,i);
    }
}

function maxHeap(arr,i){
    let left = 2*i + 1,//左孩子
        right = left + 1,//右孩子
        largest = 0;//记录最大值的索引

    if(left < arr.length && arr[left] > arr[i]){//存在左孩子且大于根
        largest = left;//最大为左孩子
    }else largest = i;//否则为根

    if(right < arr.length && arr[right] > arr[largest]){//存在右孩子且大于根
        largest = right;//最大为右孩子
    }
        
    if(largest !== i){//最大值不是根，把最大值换到根的位置
        [arr[largest],arr[i]] = [arr[i],arr[largest]];
        maxHeap(arr,largest);//交换后向下调整
    }
}
```


**利用堆求 Top k 问题的优势**
这种求 Top k 问题是可以使用排序来处理，但当我们需要在一个动态数组中求 Top k 元素怎么办？

动态数组可能会插入或删除元素，难道我们每次求 Top k 问题的时候都需要对数组进行重新排序吗？那每次的时间复杂度都为 O(nlogn)

这里就可以使用堆，我们可以维护一个 K 大小的小顶堆，当有数据被添加到数组中时，就将它与堆顶元素比较，如果比堆顶元素大，则将这个元素替换掉堆顶元素，然后再堆化成一个小顶堆；如果比堆顶元素小，则不做处理。这样，每次求 Top k 问题的时间复杂度仅为 O(logk)

**复杂度分析**

 - 时间复杂度：O(nlogk)，其中 n 是数组 arr 的长度。由于大根堆实时维护前 k 小值，所以插入删除都是O(logk)的时间复杂度，最坏情况下数组里 n个数都会插入，所以一共需要 O(nlogk) 的时间复杂度。
 -  空间复杂度：O(k)，因为大根堆里最多 k 个数。
 - O(nlogk)的时间复杂度使最大/小堆适合处理海量数据，不改变原有数据，只要容纳k的辅助空间，适合n比较大k比较小的问题。
