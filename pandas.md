# pandas tips

## 所有列转 str

- 读取时指定数据类型

  ```python
  df = pd.read_csv('test.csv', sep='|', dtype='str')
  ```

- 使用转换函数

  - 将所有列转换为字符串

  ```python
  df = df.astype(str)
  ```

  - 将某列转换为字符串

    ```python
    df = df.astype({'col1': 'float', 'col2': 'str'})
    ```

- 使用 dataframe 的 apply 或 map 函数

  ```python
  df['col1'] = df['col1'].map(lambda x: float(x))
  ```

> [from](https://blog.csdn.net/qq_34490873/article/details/81205523)

## 所有列做处理

- `dataframe.columns`

```python
df.columns = df.columns.str.replace(r'\s+', '_', regex=True)
```

- `dataframe.astype`

- `dataframe.apply`

```python
# pd.to_numberic 只能作用到单列

# 利用 apply 将它作用到整个 dataframe, 遇到错误时忽略, 不予转换该列
df = df.apply(pd.to_numberic, errors='ignore')

# 遇到错误时转换成 nan
df = df.apply(pd.to_numberic, errors='coerce')

# 遇到错误时报错
df = df.apply(pd.to_numberic, errors='raise')
```

> [from](https://blog.csdn.net/Python_Ai_Road/article/details/81158376)
