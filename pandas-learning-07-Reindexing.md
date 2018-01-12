
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
    })

df.head(5)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>C</th>
      <th>x</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016-01-01</td>
      <td>Medium</td>
      <td>0.0</td>
      <td>0.086927</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2016-01-02</td>
      <td>High</td>
      <td>1.0</td>
      <td>0.363968</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-01-03</td>
      <td>Medium</td>
      <td>2.0</td>
      <td>0.839580</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2016-01-04</td>
      <td>Low</td>
      <td>3.0</td>
      <td>0.122130</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2016-01-05</td>
      <td>Medium</td>
      <td>4.0</td>
      <td>0.393594</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 更新DataFrame索引
df_reindexed = df.reindex(index=[0,2,5],columns=['A','C','B'])

# 打印结果可以看出,df_reindexed是df实例集合中的子集
# 'B'列不存在，所有元素均为NaN
df_reindexed
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>C</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016-01-01</td>
      <td>Medium</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-01-03</td>
      <td>Medium</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2016-01-06</td>
      <td>Medium</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### 2. 重建索引以便与其他对象对齐  
有时候可能希望将一个对象轴的索引标记转换为与另一个对象相同的相同，这时候可以调用reindex_like(other_obj)简化操作


```python
df1 = pd.DataFrame(np.random.randn(5,3),columns=['col1','col2','col3'])
df2 = pd.DataFrame(np.random.randn(3,2),columns=['col1','col3'])

df1
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
      <th>col2</th>
      <th>col3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.316469</td>
      <td>-0.087153</td>
      <td>0.608960</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2.386163</td>
      <td>0.178221</td>
      <td>1.025812</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-1.733944</td>
      <td>0.370595</td>
      <td>-0.194925</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.268836</td>
      <td>0.201425</td>
      <td>-0.395783</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-1.328632</td>
      <td>-1.378002</td>
      <td>-0.347446</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
      <th>col3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.963156</td>
      <td>-0.841239</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3.062340</td>
      <td>0.914750</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.670868</td>
      <td>0.247850</td>
    </tr>
  </tbody>
</table>
</div>




```python
df3 = df1.reindex_like(df2)

# 从结果可以看出变换后的df1与df2拥有相同的索引与列
df3
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
      <th>col3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.316469</td>
      <td>0.608960</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2.386163</td>
      <td>1.025812</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-1.733944</td>
      <td>-0.194925</td>
    </tr>
  </tbody>
</table>
</div>



### 3. 重建索引时填充参数
- pad/ffill -- 向前填充值
- bfill/backfill -- 向后填充值
- nearest -- 最近索引值填充


```python
df1 = pd.DataFrame(np.random.randn(6,3),columns=['col1','col2','col3'])
df2 = pd.DataFrame(np.random.randn(2,3),columns=['col1','col2','col3'])

# 自动填充NaN
df3 = df2.reindex_like(df1)
df3
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
      <th>col2</th>
      <th>col3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.628605</td>
      <td>0.351178</td>
      <td>-1.489034</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-2.557107</td>
      <td>-0.059435</td>
      <td>-0.324165</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 现在对NaN进行前向填充处理的值
# 这里打印的效果等价于method='nearest'
print 'DataFrame with Forward Fill:'
df3 = df2.reindex_like(df1,method='ffill') 
df3
```

    DataFrame with Forward Fill:
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
      <th>col2</th>
      <th>col3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.628605</td>
      <td>0.351178</td>
      <td>-1.489034</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-2.557107</td>
      <td>-0.059435</td>
      <td>-0.324165</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-2.557107</td>
      <td>-0.059435</td>
      <td>-0.324165</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-2.557107</td>
      <td>-0.059435</td>
      <td>-0.324165</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-2.557107</td>
      <td>-0.059435</td>
      <td>-0.324165</td>
    </tr>
    <tr>
      <th>5</th>
      <td>-2.557107</td>
      <td>-0.059435</td>
      <td>-0.324165</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 如果要限制填充范围，可添加参数limit
print 'DataFrame with Forward Fill(only 2 indexes row):'
df3 = df2.reindex_like(df1,method='ffill',limit=2)
df3
```

    DataFrame with Forward Fill(only 2 indexes row):
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
      <th>col2</th>
      <th>col3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.628605</td>
      <td>0.351178</td>
      <td>-1.489034</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-2.557107</td>
      <td>-0.059435</td>
      <td>-0.324165</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-2.557107</td>
      <td>-0.059435</td>
      <td>-0.324165</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-2.557107</td>
      <td>-0.059435</td>
      <td>-0.324165</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 使用另一个对象的索引
print 'Using index of other object:'
df3 = df2.reindex(df1.index,method='nearest')
df3
```

    Using index of other object:
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
      <th>col2</th>
      <th>col3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.628605</td>
      <td>0.351178</td>
      <td>-1.489034</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-2.557107</td>
      <td>-0.059435</td>
      <td>-0.324165</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-2.557107</td>
      <td>-0.059435</td>
      <td>-0.324165</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-2.557107</td>
      <td>-0.059435</td>
      <td>-0.324165</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-2.557107</td>
      <td>-0.059435</td>
      <td>-0.324165</td>
    </tr>
    <tr>
      <th>5</th>
      <td>-2.557107</td>
      <td>-0.059435</td>
      <td>-0.324165</td>
    </tr>
  </tbody>
</table>
</div>



### 4. set_index()和reset_index()
set_index()函数的作用是将一个或多个现有列设置为DataFrame索引（行标签），默认产生一个新的对象。
使用[1]中的DataFrame举例如下：


```python
df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>C</th>
      <th>x</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016-01-01</td>
      <td>Medium</td>
      <td>0.0</td>
      <td>0.086927</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2016-01-02</td>
      <td>High</td>
      <td>1.0</td>
      <td>0.363968</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-01-03</td>
      <td>Medium</td>
      <td>2.0</td>
      <td>0.839580</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2016-01-04</td>
      <td>Low</td>
      <td>3.0</td>
      <td>0.122130</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2016-01-05</td>
      <td>Medium</td>
      <td>4.0</td>
      <td>0.393594</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 使用一列创建索引
df4 = df.set_index('A')
df4.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>C</th>
      <th>x</th>
      <th>y</th>
    </tr>
    <tr>
      <th>A</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016-01-01</th>
      <td>Medium</td>
      <td>0.0</td>
      <td>0.086927</td>
    </tr>
    <tr>
      <th>2016-01-02</th>
      <td>High</td>
      <td>1.0</td>
      <td>0.363968</td>
    </tr>
    <tr>
      <th>2016-01-03</th>
      <td>Medium</td>
      <td>2.0</td>
      <td>0.839580</td>
    </tr>
    <tr>
      <th>2016-01-04</th>
      <td>Low</td>
      <td>3.0</td>
      <td>0.122130</td>
    </tr>
    <tr>
      <th>2016-01-05</th>
      <td>Medium</td>
      <td>4.0</td>
      <td>0.393594</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 可以将索引名称设置为None，去掉'A'
df4.index.name = None
df4.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>C</th>
      <th>x</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016-01-01</th>
      <td>Medium</td>
      <td>0.0</td>
      <td>0.086927</td>
    </tr>
    <tr>
      <th>2016-01-02</th>
      <td>High</td>
      <td>1.0</td>
      <td>0.363968</td>
    </tr>
    <tr>
      <th>2016-01-03</th>
      <td>Medium</td>
      <td>2.0</td>
      <td>0.839580</td>
    </tr>
    <tr>
      <th>2016-01-04</th>
      <td>Low</td>
      <td>3.0</td>
      <td>0.122130</td>
    </tr>
    <tr>
      <th>2016-01-05</th>
      <td>Medium</td>
      <td>4.0</td>
      <td>0.393594</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 使用多列创建索引
df4 = df.set_index(['x','y'])
df4.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>A</th>
      <th>C</th>
    </tr>
    <tr>
      <th>x</th>
      <th>y</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0.0</th>
      <th>0.086927</th>
      <td>2016-01-01</td>
      <td>Medium</td>
    </tr>
    <tr>
      <th>1.0</th>
      <th>0.363968</th>
      <td>2016-01-02</td>
      <td>High</td>
    </tr>
    <tr>
      <th>2.0</th>
      <th>0.839580</th>
      <td>2016-01-03</td>
      <td>Medium</td>
    </tr>
    <tr>
      <th>3.0</th>
      <th>0.122130</th>
      <td>2016-01-04</td>
      <td>Low</td>
    </tr>
    <tr>
      <th>4.0</th>
      <th>0.393594</th>
      <td>2016-01-05</td>
      <td>Medium</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
