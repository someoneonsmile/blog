# 推进式分页扫描模式

|    date    |     tag     |
|    ---     |     ---     |
| 2021-01-10 | no-gap-scan |

## 描述

没有空隙式的扫描

处理当前页，直到当前页没有符合条件的记录，再扫描下一页

## 伪代码

```python
def scan() :
    page_no = 1
    page_size = 10
    while True :
        records = scan_page(page_no, page_size)
        if not records :
            break

        # 筛选目标记录
        targets = records.filter(condition)

        # 处理目标记录为非目标
        deal_to_not_target(targets)

        # 如果没有符合条件的记录，取下一页
        if not targets :
            page_no++
```
