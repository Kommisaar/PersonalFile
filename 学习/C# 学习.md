# C# 学习

## 接口与抽象类

## 设计模式

### 装饰器模式

### 工厂模式

### 观察者模式

### 策略模式

## 排序

### 快速排序的实现

#### 递归方式

##### 快速排序函数的实现

```c#
 // 快速排序函数
public void QuickSort(int[] array, int low, int high)
{
    if (low < high)
    {
        // pi 是分区索引，arr[pi] 现在在正确的位置
        int pi = Partition(array, low, high);
		// 递归地分别对分区前后的数组进行排序
        QuickSort(array, low, pi - 1); // 递归排序左子数组
        QuickSort(array, pi + 1, high);// 递归排序右子数组
    } 
}
```

##### 分区函数的实现

分区函数逻辑

1. 选择最后一个元素做基准`pivot`。**如果需要选择其他元素做基准，则先让该元素和最后一个元素交换即可**
2. 选择一个小于基准元素的索引`i = low - 1`，在开始遍历数组之前，`i` 被初始化为 `l - 1`，表示还没有遇到任何小于基准的元素。`low` 是当前分区子数组的起始索引，因此 `low - 1` 是一个安全的起始点，因为它位于子数组索引的有效范围之外。**i实际上是指向下一个存放小于基准元素`pivot`的元素位置的指针**
3. 开始从分区低位`low`遍历该分区，在遍历数组时，每当遇到一个小于或等于基准的元素，`i` 就会增加 1，并将该元素与索引 `i` 处的元素交换。这样做是为了确保所有小于基准的元素都被移动到数组的左侧。
4. 当遍历完成后，所有小于基准的元素都位于索引 `l` 到 `i` 的范围内。此时，`i` 是小于基准的元素的最后一个索引，因此基准元素应该放在 `i + 1` 的位置上，这样就能保证基准左边的所有元素都不大于基准，而基准右边的所有元素都不小于基准。
5. 通过将基准元素与 `arr[i + 1]` 交换，基准元素被放置在其最终位置上，这个位置也是新的分区点，它将数组划分为两个子数组。

```c#
// 分区函数
private int Partition(int[] array, int low, int high)
{
    //如果需要选择中间元素
    //int mid = low+(high-low)/2
    //(array[high],array[mid])=(array[mid],array[high])
    int pivot = array[high];// 选择数组最后一个元素作为基准（pivot）
    
    int i = low - 1; // i 是小于基准的元素的最后一个索引
	// 从 j 到 high 遍历数组元素
    for (int j = low; j < high; j++)
    {
        // 如果当前元素小于基准***如果需要降序排列修改这里的判定
        if (array[j] < pivot)
        {
            // 增加小于基准的元素的索引
            i++; 
            // 交换 arr[i] 和 arr[j]
            (array[j], array[i]) = (array[i], array[j]);
        }
    }
    // 将基准元素放到正确的位置（在小于基准的元素之后）
    (array[high], array[i+1]) = (array[i + 1], array[high]);
    // 返回分区索引
    return i + 1;
}
//int[] array = {95, 79, 7, 44, 61, 48, 41, 58, 58, 26, 63, 61, 76, 76, 67, 32, 48, 47, 17, 21}
//low = 0 high =19
//low<high
//开始分区函数(0,19)
//pivot = array[high] = 21
//i = low -1 = -1
//开始循环 j = low =0
//j = 0 array[j]=95>pivot=21 不做处理
//j = 1 array[j]=79>pivot=21 不做处理
//j = 2 array[j]=7<pivot=21 i+1=0,交换array[i]=95与array[j]=7,当前数组{7, 79, 95, 44, 61, 48, 41, 58, 58, 26, 63, 61, 76, 76, 67, 32, 48, 47, 17, 21}
//j = 3 array[j]=44>pivot=21 不做处理
//j = 4 array[j]=61>pivot=21 不做处理
//......
//j=18	array[18]=17<pivot=21 i+1=1,交换array[i]=79与array[j]=17,当前数组{7, 17, 95, 44, 61, 48, 41, 58, 58, 26, 63, 61, 76, 76, 67, 32, 48, 47, 79, 21}
//......
//循环结束
//交换array[i+1=2]=95和array[high=19]=21,{7, 17, 21, 44, 61, 48, 41, 58, 58, 26, 63, 61, 76, 76, 67, 32, 48, 47, 79, 95}
//返回分区索引i+1 = 2

	//开始QuickSort函数array,low = 0 high=pi-1=1
    //low<high
    //开始分区函数(0,1),array = {7, 17}
    //pivot = array[high] = 17
    //i = low -1 =-1
    //开始循环 j =low =0
    //j = 0 array[j]=7<pivot=17,i+1 =0,交换array[i]=7与array[j]=7,当前数组{7, 17}
    //j = 1 array[j]=17=pivot =17 ,不做处理
    //循环结束
	//交换array[i+1=1]=17与array[high=1]=17
	//返回分区索引i+1 = 1

		//开始QuickSort函数array,low = 0 high=pi-1=0
		//low==high
		//结束

	//开始QuickSort函数array,low = 3 high=19
	//low<high
	//开始分区函数(3,19),array = {44, 61, 48, 41, 58, 58, 26, 63, 61, 76, 76, 67, 32, 48, 47, 79, 95}
	//pivot = array[high] = 95
	//i = low -1 = 2
	//开始循环 j = low = 3
    //j = 3 array[j]=44<pivot=95,i+1 = 3,交换array[i=3]=44与array[j=3]=44,当前数组{44, 61, 48, 41, 58, 58, 26, 63, 61, 76, 76, 67, 32, 48, 47, 79, 95}
    //j = 4 array[j]=61<pivot=95,i+1 = 4,交换array[i=4]=61与array[j=4]=61,当前数组{44, 61, 48, 41, 58, 58, 26, 63, 61, 76, 76, 67, 32, 48, 47, 79, 95}
    //......
    //j=18 array[j]=79<pivot=95,i+1 = 18,交换array[i=18]=79与array[j=18]=79,当前数组{44, 61, 48, 41, 58, 58, 26, 63, 61, 76, 76, 67, 32, 48, 47, 79, 95}
    //j=19 array[j]=95=pivot=95,不做处理
	//退出循环
    //交换array[i+1=19]=95和array[high=95]
    //返回分区索引i+1 = 19

        //开始QuickSort函数array,low = 3 high=18
        //......
			//开始QuickSort函数array,low = 3 high=17
			//low<high
			//开始分区函数(3,17),array= {44, 61, 48, 41, 58, 58, 26, 63, 61, 76, 76, 67, 32, 48, 47}
			//pivot = array[high]
			//i = low -1 =2
            //开始循环j=low =3
            //j = 3 array[j]=44<47 i+1=3,交换array[i=3]=44和array[j=3]=44,当前数组{44, 61, 48, 41, 58, 58, 26, 63, 61, 76, 76, 67, 32, 48, 47}
            //j = 4 array[j]=61>pivot 不做处理
            //j = 5 array[j]=48>pivot 不做处理
            //j = 6 array[j]=41<pivot i+1=4,交换array[i=4]=61和array[j=6]=41,当前数组{44, 41, 48, 61, 58, 58, 26, 63, 61, 76, 76, 67, 32, 48, 47}
            //j = 7 array[j]=58>pivot 不做处理
            //j = 8 array[j]=58>pivot 不做处理
            //j = 9 array[j]=26<pivot i+1=5,交换array[i=5]=48和array[j=9]=26,当前数组{44, 41, 26, 61, 58, 58, 48, 63, 61, 76, 76, 67, 32, 48, 47}
            //......
            //j = 15 array[j] 32<pivot=47 i+1=6,交换array[i=6]=61和array[j=15]=28,当前数组{44, 41, 26, 28, 58, 58, 48, 63, 61, 76, 76, 67, 61, 48, 47}
			//退出循环
			//交换array[i+1=7]=58和array[high]=47,当前数组{44, 41, 26, 28, 47, 58, 48, 63, 61, 76, 76, 67, 61, 48, 58}
			//返回分区索引i+1=7
//继续其他分区的......

```



#### 非递归方式

使用栈进行非递归操作

##### 使用栈进行迭代

```c#
public void QuickSortWithStack(int[] arr, int l, int h)
    {
        // 创建一个栈用于存储起始和结束索引
        Stack<int> stack = new Stack<int>();

        // 将初始值推入栈中
        stack.Push(l);
        stack.Push(h);

        // 循环直到栈为空
        while (stack.Count > 0)
        {
            // 弹出 h 和 l
            h = stack.Pop();
            l = stack.Pop();

            // 设置分区索引
            int p = Partition(arr, l, h);

            // 如果有元素在分区索引左边，将它们的索引推入栈中
            if (p - 1 > l)
            {
                stack.Push(l);
                stack.Push(p - 1);
            }

            // 如果有元素在分区索引右边，将它们的索引推入栈中
            if (p + 1 < h)
            {
                stack.Push(p + 1);
                stack.Push(h);
            }
        }
    }
```

#### 三指针快速排序

## KMP算法
