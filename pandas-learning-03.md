
## pandas学习笔记03

### pandas.Panel结构
pandas.Panel(data,items,major_axis,minor_axis,dtype,copy)
Panel是具有异构数据的三维数据结构，以三维数组形式存储，一般很难用图形描述出来。
简单来说，panel就是一系列DataFrame对象的容器。

### 1. 创建Panel
- 从ndarrays创建
- 从DataFrames字典创建

#### 创建一个空Panel


```python
import pandas as pd
p = pd.Panel()
print p
```

    <class 'pandas.core.panel.Panel'>
    Dimensions: 0 (items) x 0 (major_axis) x 0 (minor_axis)
    Items axis: None
    Major_axis axis: None
    Minor_axis axis: None
    

#### 从3维ndarray创建


```python
import pandas as pd
import numpy as np

data = np.random.rand(2,4,5)
p = pd.Panel(data)
print p
```

    <class 'pandas.core.panel.Panel'>
    Dimensions: 2 (items) x 4 (major_axis) x 5 (minor_axis)
    Items axis: 0 to 1
    Major_axis axis: 0 to 3
    Minor_axis axis: 0 to 4
    

#### 从DataFrame对象字典创建


```python
import pandas as pd
import numpy as np
data = {'Item1':pd.DataFrame(np.random.randn(4,3)),
       'Item2':pd.DataFrame(np.random.randn(4,2))}
p = pd.Panel(data)
print p
```

    <class 'pandas.core.panel.Panel'>
    Dimensions: 2 (items) x 4 (major_axis) x 3 (minor_axis)
    Items axis: Item1 to Item2
    Major_axis axis: 0 to 3
    Minor_axis axis: 0 to 2
    

### 2. 从Panel选择数据

从panel选择数据使用：
- Items
- Major_axis
- Minor_axis

#### 使用Items


```python
import pandas as pd
import numpy as np
data = {'Item1':pd.DataFrame(np.random.randn(4,3)),
       'Item2':pd.DataFrame(np.random.randn(4,2))}
p = pd.Panel(data)
print p['Item1']
```

              0         1         2
    0  0.157014 -0.709222 -0.059992
    1 -2.148879  0.098699  1.220792
    2  0.050344  0.528144 -0.759837
    3 -1.100057  1.875480 -1.085946
    

##### 使用major_axis, minor_axis


```python
import pandas as pd
import numpy as np
data = {'Item1' : pd.DataFrame(np.random.randn(4, 3)), 
        'Item2' : pd.DataFrame(np.random.randn(4, 2))}
p = pd.Panel(data)
print p['Item1'],'\n',p['Item2'],'\n'
print p.major_xs(0),'\n' # 0 表示行索引
print p.minor_xs(2) # 2 表示列索引
```

              0         1         2
    0  0.863158 -0.279584 -0.268545
    1 -0.347709 -1.170371 -1.959802
    2 -0.849025 -1.397576  0.851528
    3 -1.091824  0.475429 -0.235250 
              0         1   2
    0 -0.543914 -0.702961 NaN
    1  1.144517  1.324323 NaN
    2  0.023756 -0.937695 NaN
    3  1.457539  0.978607 NaN 
    
          Item1     Item2
    0  0.863158 -0.543914
    1 -0.279584 -0.702961
    2 -0.268545       NaN 
    
          Item1  Item2
    0 -0.268545    NaN
    1 -1.959802    NaN
    2  0.851528    NaN
    3 -0.235250    NaN
    
