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
