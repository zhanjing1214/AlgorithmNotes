### Binary Search



##### 传统二分模板

传统二分一般是让你在给定数组中找到任意/第一/最后一个满足某个条件的位置，这个时候使用二分法代码基本相同，就是判断的时候稍微变动。

```java
// 前面先判断一些edge case, 比如数组会否为空什么的
if (nums == null || nums.length == 0) {
  return -1;
}

//初始化start和end
int start = 0, end = nums.length - 1;
while (start + 1 < end) { //注意条件 跳出时start和end都要判断一下
  int mid = start + (end - start) / 2; //这样写防止溢出
  //1. 若是任意一个满足，则=时就return该值
  //2. 若是第一个满足，则=时end=mid，往前缩
  //3. 若是最后一个满足，则=时start=mid，往后缩
  if (nums[mid] >= target) { //第2种情况
    end = mid;
  } else {
    start = mid;
  }
}
//出来之后判断一下start和end
if (nums[start] == target) { //因为找第一个所以先判断前面的是不是
  return start;
}
if (nums[end] == target) {
  return end;
}
return -1;
```



##### 二分位置

这一类题目的条件并不是匹配某个值，但是有其他条件供你排除某一办，比如FindPeakElement。这一类的题目大致与上面模板一样，就是要搞清楚中间那一部分的逻辑关系，大了去哪边，等于属于那一边，出来后再判断一下。

```java
//写一道比较经典的Search in rotated sorted array,该题相当于结合了传统与二分位置的题目,强调的是数组中不存在重复元素
public int findMin(int[] nums, int target) {
  //依旧是先判断edge case
  if (nums == null || nums.length == 0) {
    return -1;
  }
  int start = 0, end = nums.length -1;
  while (start + 1 < end) {
    int mid = start + (end - start) / 2;
    if (nums[mid] == target) {
      return mid;
    }
    if (nums[mid] > nums[end]) { //前半部分顺序增长，最小值最大值都在后半部分
      if (nums[start] <= target && nums[mid] > target) {
        end = mid;
      } else {
        start = mid;
      }
    } else { //否则后半部分顺序增长，最小值最大值都在前半部分
      if (nums[mid] < target && nums[end] >= target) {
        start = mid;
      } else {
        end = mi;
      }
    }
  }
  if (nums[start] == target) {
    return start;
  }
  if (nums[end] == target) {
    return end;
  }
  return -1;
}
```





#####二分答案之看不出来的二分法

题目可能并没有数组，但是会要求你找到满足条件的最大值或最小值，这里还是有点蒙圈，感觉逻辑比较难处理。

例题如:

Sqrtx: http://www.lintcode.com/problem/sqrtx/

Wood cut: http://www.lintcode.com/problem/wood-cut/

Copy books: http://www.lintcode.com/en/problem/copy-books/

其实难点就是二分的时候要判断什么时候才是符合条件的，就是要把问题转化为二分的思想。





##### 三步翻转

这个方法可以用来解rotate序列的情况，非常方便，三步就可以把rotate序列还原，同理也可以把顺序结构rotate。

[<u>4,5</u>,1,2,3] → [5,4,<u>1,2,3</u>] → [<u>5,4,3,2,1</u>] → [1,2,3,4,5]  //下划线表示要rotate的元素




