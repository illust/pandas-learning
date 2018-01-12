
# 重建索引

### 1. 重建索引改变DataFrame的行列标签，获得指定行列的集合
#### reindex功能可以帮助实现很多操作
- 对现有数据重新排序以匹配一组新的标签
- 在缺少标签数据的标签位置插入缺失值（NA）标记


```python
import pandas as pd
import numpy as np

N = 20

df = pd.DataFrame({
        'A':pd.date_range(start='2016-01-01',periods=N,freq='D'),
        'x':np.linspace(0,stop=N-1,num=N),
        'y':np.random.rand(N),
        'C':np.random.choice(['Low','Medium','High'],N).tolist(),
        'D':np.random.normal(100,10,size=(N)).tolist()
    })

print df.head(5),'\n'

# 更新DataFrame索引
df_reindexed = df.reindex(index=[0,2,5],columns=['A','C','B'])

# 打印结果可以看出,df_reindexed是df实例集合中的子集
# 'B'列不存在，所有元素均为NaN
print df_reindexed
```

               A       C           D    x         y
    0 2016-01-01    High  104.072533  0.0  0.609699
    1 2016-01-02  Medium   93.026062  1.0  0.596664
    2 2016-01-03     Low  123.443744  2.0  0.218312
    3 2016-01-04     Low   98.106870  3.0  0.806325
    4 2016-01-05  Medium  105.314873  4.0  0.237080 
    
               A       C   B
    0 2016-01-01    High NaN
    2 2016-01-03     Low NaN
    5 2016-01-06  Medium NaN
    

### 2. 重建索引以便与其他对象对齐  
有时候可能希望将一个对象轴的索引标记转换为与另一个对象相同的相同，这时候可以调用reindex_like(other_obj)简化操作


```python
df1 = pd.DataFrame(np.random.randn(5,3),columns=['col1','col2','col3'])
df2 = pd.DataFrame(np.random.randn(3,2),columns=['col1','col3'])

print df1,'\n'
print df2,'\n'

df1 = df1.reindex_like(df2)
# 打印结果，可以看出变换后的df1与df2拥有相同的索引与列
print df1
```

           col1      col2      col3
    0 -1.505201 -0.612742 -2.429520
    1 -0.018661  0.792403 -0.680863
    2 -1.668620  1.413393 -2.301443
    3 -0.887630 -0.426674  0.938337
    4  0.278087  0.815155 -0.560904 
    
           col1      col3
    0 -1.540826  0.147626
    1 -1.532213  1.566316
    2  0.493615 -0.550070 
    
           col1      col3
    0 -1.505201 -2.429520
    1 -0.018661 -0.680863
    2 -1.668620 -2.301443
    

### 3. 重建索引时填充参数
- pad/ffill -- 向前填充值
- bfill/backfill -- 向后填充值
- nearest -- 最近索引值填充


```python
df1 = pd.DataFrame(np.random.randn(6,3),columns=['col1','col2','col3'])
df2 = pd.DataFrame(np.random.randn(2,3),columns=['col1','col2','col3'])

# 自动填充NaN
print df2.reindex_like(df1),'\n'

# 现在对NaN进行填充处理的值
# 这里打印的效果等价于method='nearest'
print 'DataFrame with Forward Fill:'
print df2.reindex_like(df1,method='ffill') ,'\n'

# 如果要限制填充范围，可添加参数limit
print 'DataFrame with Forward Fill(only 2 indexes row):'
print df2.reindex_like(df1,method='ffill',limit=2),'\n'

# 使用另一个对象的索引
print 'Useing reindex:'
print df2.reindex(df1.index,method='nearest')
```

           col1      col2      col3
    0 -1.101559 -1.549425 -2.061898
    1  0.819264  0.704654 -0.353501
    2       NaN       NaN       NaN
    3       NaN       NaN       NaN
    4       NaN       NaN       NaN
    5       NaN       NaN       NaN 
    
    DataFrame with Forward Fill:
           col1      col2      col3
    0 -1.101559 -1.549425 -2.061898
    1  0.819264  0.704654 -0.353501
    2  0.819264  0.704654 -0.353501
    3  0.819264  0.704654 -0.353501
    4  0.819264  0.704654 -0.353501
    5  0.819264  0.704654 -0.353501 
    
    DataFrame with Forward Fill(only 2 indexes row):
           col1      col2      col3
    0 -1.101559 -1.549425 -2.061898
    1  0.819264  0.704654 -0.353501
    2  0.819264  0.704654 -0.353501
    3  0.819264  0.704654 -0.353501
    4       NaN       NaN       NaN
    5       NaN       NaN       NaN 
    
    Useing reindex:
           col1      col2      col3
    0 -1.101559 -1.549425 -2.061898
    1  0.819264  0.704654 -0.353501
    2  0.819264  0.704654 -0.353501
    3  0.819264  0.704654 -0.353501
    4  0.819264  0.704654 -0.353501
    5  0.819264  0.704654 -0.353501
    


```python

```
