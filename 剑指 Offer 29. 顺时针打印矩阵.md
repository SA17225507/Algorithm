

> 输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。
> 
>  
> 
> 示例 1：
> 
> 输入：matrix = [[1,2,3],[4,5,6],[7,8,9]] 
> 输出：[1,2,3,6,9,8,7,4,5] 
> 示例 2：
> 
> 输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
> 输出：[1,2,3,4,8,12,11,10,9,5,6,7]

思路：
什么时候继续打印下一圈？也就是要找到循环的条件。
什么时候结束循环呢？我们注意到一个4x4的矩阵，只有两圈，从第一圈到第二圈，打印的起点从（0，0）变成了（1，1），我们发现4>1*2，而当坐标为（2，2）时，4>2*2就不成立了，而此时（2，2）位置的数字已经在第二圈被打印过。类似的对于5*5矩阵，最后一圈只有一个数字，起点坐标为（2，2），满足5>2*2，同理其他矩阵也类似，由此我们发现结束循环的条件是col>start*2并且row>start*2。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201125170544883.png#pic_center)
实现打印一圈，注意边界条件。

 1. 从左到右，从起始列号到终止列号。
 2. 从上到下，终止行号大于起始行号，因为如果不大于，说明只有一行，从左到右已经打印过。
 3. 从右到左，终止行号大于起始行号且终止列号大于起始列号，同理，不大于只有一列。
 4. 从下到上，终止列号大于起始列号且终止行号比起始行号至少大2，因为不大2的话，例如上图的第二圈，从左到右已经打印过6，从右到左已经打印过10，所以从下到上无需打印任何元素。
 
 

```javascript
/**
 * @param {number[][]} matrix
 * @return {number[]}
 */
var spiralOrder = function(matrix) {
    if(matrix == null || matrix.length == 0 || matrix[0].length == 0) return [];
    let res = [],start = 0;
    const row = matrix.length,col = matrix[0].length;
    let endX = col - start - 1,
        endY = row - start - 1;

    while(row > start*2 && col > start*2){
        for(let i = start;i<=endX;i++){
            res.push(matrix[start][i]);
        }
        if(endY > start){
            for(let i = start+1;i<=endY;i++){
                res.push(matrix[i][endX]);
            }
        }
        if(endY > start && endX > start){
            for(let i = endX-1;i>=start;i--){
                res.push(matrix[endY][i]);
            }
        }
        if(endY > start+1 && endX > start){
            for(let i = endY-1;i>start;i--){
                res.push(matrix[i][start]);
            }
        }
        start++;
        endX--;
        endY--;
    }
    return res;
};
```
复杂度分析：
时间复杂度：O(mn)
空间复杂度：O(1)