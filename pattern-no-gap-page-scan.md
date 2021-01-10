# 推进式分页扫描模式

|    date    |     tag     |
|    ---     |     ---     |
| 2021-01-10 | no-gap-scan |

## 描述

没有空隙式的扫描

处理当前页，直到当前页没有符合条件的记录，再扫描下一页

适用场景：分页条件里不能直接用特殊条件，或者要进行进一步的筛选

## 伪代码

```python
def scan() :
    page_no = 1
    page_size = 10
    while True :
        # 根据条件筛选
        records = scan_page(condition, page_no, page_size)
        if not records :
            break

        # 根据特殊条件筛选目标记录
        targets = records.filter(other_condition)

        # 处理目标记录为非目标
        deal_to_not_target(targets)

        # 如果没有符合条件的记录，取下一页
        if not targets :
            page_no++
```
