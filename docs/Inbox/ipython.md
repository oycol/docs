## Ipython shell命令

### %quickref

| 命令        | 参数                                 | 描述                                  |
| ----------- | ------------------------------------ | ------------------------------------- |
| %somemagic? |                                      | read its docstring                    |
| %lsmagic    |                                      | see all the available magic functions |
| %autocall 1 | starting IPython with `--autocall 1` | Auto-parentheses                      |
|             |                                      | Auto-Quoting                          |

#### 文本编辑

| 命令          | 描述                                                |
| ------------- | --------------------------------------------------- |
| Ctrl-p/Ctrl-n | 选择之前的命令                                      |
| Ctrl Shift  - | 撤销                                                |
| _i            | stores previous input                               |
| _ii           | next previous                                       |
| _iii          | next-next previous                                  |
| _ih           | a list of all input _ih[n] is the input from line n |
| _             |                                                     |
| __            |                                                     |
| ___           |                                                     |

#### 光标移动

| 命令   | 描述               |
| ------ | ------------------ |
| Ctrl u | 删除光标到最前     |
| Ctrl k | 删除光标到最后     |
| Ctrl x | 移动光标最前或最后 |
|        |                    |