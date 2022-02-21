---
title: "如何分析你的算法"
date: 2021-07-10T16:57:04+08:00
---
众所周知，数据结构和算法要解决的问题是“省”和“快”，即如何让代码运行的更快，如何让代码能更省存储空间
### 复杂度分析
需要从两个维度分析复杂度
- 时间复杂度
- 空间复杂度
```
func cal(n int) int {
    sum := 0
    for i := 1;i <= n;i++ {
        sum = sum + i
    }

    return sum
}
```
例子中第二行是常量的执行时间，与n的大小无关，第3，4行代码执行了n次。
分析代码的执行时间，假设每一行都需要1个unit_time，那这段代码总的执行时间就是(2n+1)*unit_time,所有代码的执行时间T(n)与每行代码的执行次数成正比
大O时间复杂度实际上并不能表示真正执行的时间，是表示代码执行时间随数据增长的变化趋势，所以时间复杂度是T(n) = O(n)

##### 如何计算时间复杂度

1. 关注循环执行次数最多的一段代码

上面说到大O表示法是表示一种变化趋势，所以只需要记录一个最大的量级，所以只关注循环执行最多的那一段代码就可以了

2. 加法法则

分析代码每一部分的时间复杂度，然后放到一起，再取一个量级最大的作为整段的复杂度
3. 乘法法则

嵌套代码的复杂度等于嵌套内外代码复杂度的乘积

##### 看两个例子
```
func cal(n int) int {
    sum1 := 0
    for i := 1;i < n;i++ {
        sum1 = sum1 + i
    }

    sum2 := 0
    for q := 1;q < n;q++ {
        sum2 = sum2 + 1
    }

    sum3 := 0
    for a := 1;a < n; a++ {
        for b := 1;b < n;b++ {
            sum3 = sum3 + a + b
        }
    }

    sum := sum1 + sum2 + sum3
    return sum
}
```
使用加法法则 可以求出函数的时间复杂度 O(n)+O(n)+O(n^2) 总的时间复杂度等于量级最大的时间复杂度 O(n^2)

```
func f(n int) int {
    sum := 0
    for i := 1;i < n;i++ {
        sum = sum + i
    }

    return sum
}

func cal(n int) int {
    sum := 0
    for i := 1;i < n;i++ {
        sum = sum + f(i)
    }

    return sum
}
```
使用乘法法则 cal函数(O(n))中调用f函数(O(n))属于嵌套代码 所以时间复杂度T(n) = O(n^2)

#### 集中常见的时间复杂度
- 多项式量级 O(1) O(logn) O(n) O(nlogn) O(n^k)
- 非多项式量级 O(2^n) O(n!)

1. O(1)
```
i := 1
a := 2
sum := i + a
```
O(1)是常量级时间复杂度的一种表示方法，并不是代表只执行了一行代码

2. O(logn) O(nlogn)
```
func cal(n int) {
    i := 1
    for i < n {
        i = i * 2
    }
}
```
第四行代码是执行次数最多的代码 所以第四行的时间复杂度就是整个函数的时间复杂度 转换问题 2^x=n

x=log2n 时间复杂度O(logn)

3. O(m + n) O(m * n)
- what?? 为什么会有O(m + n) 这种时间复杂度 不是应该满足加法法则嘛
```
func cal(m int, n int) int {
    i := 1
    j := 1
    sum1 := 0
    sum2 := 0
    for ;i <= m;i++ {
        sum1 = sum1 + i
    }
    
    for ;j <= n;j++ {
        sum2 = sum2 + j
    }
    return sum1 + sum2
}
```
我们事先无法评估m、n谁的量级更大，所以在表示复杂度的时候无法省略其中一个 无法使用加法法则 上面的时间复杂度就是O(m + n)  但是乘法法则仍然成立

### 空间复杂度分析
- 什么是空间复杂度
前面说的时间复杂度是表示算法执行时间与数据规模之间的增长关系
那空间复杂度应该是表示算法的储存空间与数据规模之间的增长关系

```
func print(n int) {
   i := 0
   a := make([]int, n)
   for ;i < n;i++ {
       a[i] = i * i
   }

   for j := n-1;j >= 0;j-- {
      fmt.Println(a[j])
   }
}
```
第二行申请了一个变量i和n没有关系 O(1) 第三行申请了一个容量是n的切片 其他代码没有占用什么空间 所以空间复杂度为O(n)

------
在做算法题的时候经常会遇到几个词“最好情况的时间复杂度”“最坏情况的时间复杂度”“平均情况的时间复杂度”“均摊时间的时间复杂度” ？？这些都是代表什么意思 要怎么去算？

```
func findIndex(a []int, x int) int {
   count := len(a)
   index := -1

   for i:=0;i<count;i++ {
       if a[i] == x {
           index = i
           break
       }
   }

   return index
}
```
这段函数的时间复杂度是O(n)? 
最好情况是第一次就命中 只需要执行一次时间复杂度则为O(1),最坏情况是都没有找到全部遍历了一遍时间复杂度是O(n)

?? 平均时间复杂度又应该怎么算呢

出现的情况 在切片中 和 不在切片中 概率分别是1/2 在切片中的概率都是1/n ![image](https://user-images.githubusercontent.com/34566156/153562714-517eead7-e9d0-486b-861c-5ea7f1a3fa19.png)

(3n+1)/4 得出平均时间复杂度为O(n)