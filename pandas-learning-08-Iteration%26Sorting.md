
# 迭代与排序

## 1. 迭代

### 1.1 对DataFrame进行迭代


```python
import pandas as pd
import numpy as np

N = 20
df = pd.DataFrame({
        'A':pd.date_range(start='2016-01-01',periods=N,freq='D'),
        'x':np.linspace(0,stop=N-1,num=N),
        'y':np.random.rand(N),
        'C':np.random.choice(['low','Medium','High'],N).tolist(),
        'D':np.random.normal(100,10,size=(N)).tolist()
    })

for col in df:
    print col
```

    A
    C
    D
    x
    y
    

#### 迭代DataFrame中的所有行或列，我们可以使用：
1. iteritems()-- 对列进行迭代，返回(columns,Series)对
2. iterrows() -- 对行进行迭代，返回(index，Series)对
3. itertuples() -- 对行进行迭代，返回namedtuples类型  

**注意：**这里只是提到对DataFrame的迭代，而对Series和Panel的迭代有所不同，因为重要性比较小，这里不涉及

### 1.2 iteritems()
将每一列的名称作为键，列值作为Series对象进行迭代


```python
import pandas as pd
import numpy as np

df = pd.DataFrame(np.arange(0,12).reshape(4,3),columns=['col1','col2','col3'])
print df,'\n'
for key,value in df.iteritems():
    print key,'\n',value,isinstance(value,pd.Series),'\n'
```

       col1  col2  col3
    0     0     1     2
    1     3     4     5
    2     6     7     8
    3     9    10    11 
    
    col1 
    0    0
    1    3
    2    6
    3    9
    Name: col1, dtype: int32 True 
    
    col2 
    0     1
    1     4
    2     7
    3    10
    Name: col2, dtype: int32 True 
    
    col3 
    0     2
    1     5
    2     8
    3    11
    Name: col3, dtype: int32 True 
    
    

### 1.3 iterrows()
iterrows()返回产生每个索引值的迭代器以及包含每行数据的序列


```python
import pandas as pd
import numpy as np

df = pd.DataFrame(np.arange(0,12).reshape(4,3),columns=['col1','col2','col3'])
print df,'\n'
for row_index,row in df.iterrows():
    print row_index,row,'\n'
```

       col1  col2  col3
    0     0     1     2
    1     3     4     5
    2     6     7     8
    3     9    10    11 
    
    0 col1    0
    col2    1
    col3    2
    Name: 0, dtype: int32 
    
    1 col1    3
    col2    4
    col3    5
    Name: 1, dtype: int32 
    
    2 col1    6
    col2    7
    col3    8
    Name: 2, dtype: int32 
    
    3 col1     9
    col2    10
    col3    11
    Name: 3, dtype: int32 
    
    

**注意:** 因为iterrows()遍历行，所以不会保留行的数据类型。0,1,2是行索引，col1，col2，col3是列索引

### 1.4 itertuples()
itertuples()方法将返回一个迭代器，产生DataFrame中每一行的命名元组。元组的第一个元素将是行对应的索引值，而其余的值是行元素值  
该函数通常比iterrows()快，因为不需要做转换


```python
import pandas as pd
import numpy as np

df = pd.DataFrame(np.arange(0,12).reshape(4,3),columns=['col1','col2','col3'])
print df,'\n'
for row in df.itertuples():
    print row,isinstance(row,tuple)
```

       col1  col2  col3
    0     0     1     2
    1     3     4     5
    2     6     7     8
    3     9    10    11 
    
    Pandas(Index=0, col1=0, col2=1, col3=2) True
    Pandas(Index=1, col1=3, col2=4, col3=5) True
    Pandas(Index=2, col1=6, col2=7, col3=8) True
    Pandas(Index=3, col1=9, col2=10, col3=11) True
    

**注意：** 迭代的意义是为了便于阅读，不要尝试迭代过程中修改任何对象，因为迭代器返回的只是原始对象的拷贝

## 2. 排序

### 在pandas中，有两种排序方法：
- 按标签
- 按实际值


```python
import pandas as pd
import numpy as np

unsorted_df = pd.DataFrame(np.random.randn(5,2),index=[1,4,2,3,0],
                           columns=['col2','col1'])
print unsorted_df 
# 可以看出在unsorted_df中，标签以及元素值都是无序的
```

           col2      col1
    1  2.526863 -0.771198
    4 -0.150883 -1.130699
    2 -0.553915  1.007639
    3  0.566399 -0.465451
    0 -1.219426  1.720222
    

### 2.1 按标签
使用sort_index()方法，通过传递轴参数和排序顺序，可以对DataFrame进行排序


```python
print "原始DataFrame:",'\n'
print unsorted_df,'\n'
# 默认情况下axis=0，对行标签排序
sorted_df = unsorted_df.sort_index()
print "按行标签排序：",'\n'
print sorted_df,'\n'

# 默认情况下，索引是按升序排列的，通过设置ascending参数可以实现逆序排列
resorted_df = unsorted_df.sort_index(ascending=False) 
print "行标签逆序排序：",'\n'
print resorted_df,'\n'

# 对列名排序
sortedcol_df = unsorted_df.sort_index(axis=1)
print "按列标签排序：",'\n'
print sortedcol_df
```

    原始DataFrame: 
    
           col2      col1
    1 -1.339667  0.115424
    4 -1.037781  1.637041
    2 -1.249779 -1.191660
    3 -0.019502  0.270886
    0 -0.834618  0.086259 
    
    按行标签排序： 
    
           col2      col1
    0 -0.834618  0.086259
    1 -1.339667  0.115424
    2 -1.249779 -1.191660
    3 -0.019502  0.270886
    4 -1.037781  1.637041 
    
    行标签逆序排序： 
    
           col2      col1
    4 -1.037781  1.637041
    3 -0.019502  0.270886
    2 -1.249779 -1.191660
    1 -1.339667  0.115424
    0 -0.834618  0.086259 
    
    按列标签排序： 
    
           col1      col2
    1  0.115424 -1.339667
    4  1.637041 -1.037781
    2 -1.191660 -1.249779
    3  0.270886 -0.019502
    0  0.086259 -0.834618
    

### 2.2 按值
与索引排序不同的是，sort_values()是按值进行排序的方法。它接受一个'by'参数，该参数将使用DataFrame的列标签来对该列元素值进行排序


```python
unsorted_df = pd.DataFrame({'col1':[2,1,1,1],'col2':[1,3,2,4]})
sorted_df = unsorted_df.sort_values(by='col2')

print "原始DataFrame：",'\n',unsorted_df,'\n'
print "按col2列进行元素值的排序：",'\n',sorted_df,'\n'

# sort_values()提供多种排序算法以供选择：mergesort,heapsort,quicksort
# mergesort是唯一的稳定算法
sorted_df_ms = unsorted_df.sort_values(by='col1',kind='mergesort')
print "排序算法为合并排序：",'\n',sorted_df_ms
```

    原始DataFrame： 
       col1  col2
    0     2     1
    1     1     3
    2     1     2
    3     1     4 
    
    按col2列进行元素值的排序： 
       col1  col2
    0     2     1
    2     1     2
    1     1     3
    3     1     4 
    
    排序算法为合并排序： 
       col1  col2
    1     1     3
    2     1     2
    3     1     4
    0     2     1
    


```python

```
