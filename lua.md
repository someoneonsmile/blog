# lua 笔记

|    date    | tag |
|    ---     | --- |
| 2022-05-01 | lua |

## metamethods

- `__index`
- `__newindex`
- `__mode`
- `__call`
- `__metatable`
- `__tostring`
- `__len`
- `__pairs`
- `__ipairs`
- `__gc`
- `__name`
- `__close`

- `__unm`
- `__add`
- `__sub`
- `__mul`
- `__div`
- `__idiv`
- `__mod`
- `__pow`
- `__concat`

- `__band`
- `__bor`
- `__bxor`
- `__bnot`
- `__shl`
- `__shr`

- `__eq`
- `__it`
- `__le`

> [文档](http://lua-users.org/wiki/MetatableEvents)

自定义 ipairs pairs 示例

```lua
local M = {}

function M.__pairs(tbl)

  -- Iterator function takes the table and an index and returns the next index and associated value
  -- or nil to end iteration

  local function stateless_iter(tbl, k)
    local v
    -- Implement your own key,value selection logic in place of next
    k, v = next(tbl, k)
    if nil~=v then return k,v end
  end

  -- Return an iterator function, the table, starting point
  return stateless_iter, tbl, nil
end

function M.__ipairs(tbl)
  -- Iterator function
  local function stateless_iter(tbl, i)
    -- Implement your own index, value selection logic
    i = i + 1
    local v = tbl[i]
    if nil~=v then return i, v end
  end

  -- return iterator function, table, and starting point
  return stateless_iter, tbl, 0
end

t = setmetatable({5, 6, a=1}, M)

for k,v in ipairs(t) do
  print(string.format("%s: %s", k, v))
end
-- Prints the following:
-- 1: 5
-- 2: 6

for k,v in pairs(t) do
  print(string.format("%s: %s", k, v))
end
-- Prints the following:
-- 1: 5
-- 2: 6
-- a: 1
```

> [`__pairs` 链接](http://lua-users.org/wiki/GeneralizedPairsAndIpairs)
