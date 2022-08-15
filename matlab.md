# matlab 浅识

## 概念

### dim

`dim` 代表维度

ex: sum(A, dim):

```matlab
  A = [1, 2; 3, 4];
  B = sum(A, 1) % mean: B = A(1) + A(2)
  % B = [4, 6]
  C = sum(A, 2) % mean: C = A(:, 1) + A(:, 2)
  % C = [3; 7]
```

即: dim 维度的所有数据对应的 `(n - 1)` 维数据

### meshgrid 与 ndgrid

约定 `i` 代表任意有效数

> [X, Y, Z] = `meshgrid(x, y, z)`

`X`: size = [length(y), length(x), length(z)], 每一行都是 `x` 的副本

`Y`: size = [length(y), length(x), length(z)], 每一列都是 `y` 的副本

`Z`: size = [length(y), length(x), length(z)], Z(:, :, i) = z(i)

> `[X1, X2, ..., Xn] = ndgrid(x1, x2, ..., xn)`

`xn`: 在 `dim=n` 的坐标向量

`Xn`: size = [length(x1), length(x2), ..., length(xn)], Xn(:, :, ..., i) = xn(i)
