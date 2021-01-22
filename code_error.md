此篇用于统计如何更好地理解代码

*************************************

*某个具体的点*

```python
import pandas as pd         
train_df = pd.read_csv("something.csv",sep = "\t",nrows = 100)
train_df["text_len"] = train_df["text"].apply(lambda x : len(x.split(" ")))
```

这个时候最优解应该是**csdn**一波，建议寻找*2-3个*，不要试图翻书或者官方文档，你会发现找半天都没有你想要的

