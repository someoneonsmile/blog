# fish shell

|    date    | tag  |
|    ---     | ---  |
| 2022-05-14 | fish |

## fn

- random
  ```
	random

	# random SEED

	# random START END

	# random START STEP END

	# random choice [ITEMS ...]
	open (random choice **.jpg)
  ```

- open

	open with default soft

- realpath

	convert a path to an absolute path without symlinks

- read

    ```
    # read line of input into variables
    read read_input

    # 设置默认输入值
    -c CMD or --command CMD

    # makes the variables global
    -g

    # 加密输入
    -s or --silent

    # prompt
    -p or --prompt PROMPT_CMD

    -P or --prompt-str

    ex: mysql -uuser -p(read)
    ```

- psub

    管道内容写入到临时文件, 并返回文件名

    ```
    echo (echo 12 | psub)
    # output: /tmp/.psub.MpKihKgXXM

    # -s or --suffix
    echo (echo 12 | psub -s .txt)
    # output: /tmp/.psub.MpKihKgXXM.txt
    ```
