#### [1370. 上升下降字符串](https://leetcode-cn.com/problems/increasing-decreasing-string/) - 桶计数

>   2020/11/25

解题思路 构造包含所有字符串的桶 解得字符串的数量并遍历 排序拼接

??? "我的解题"

````python
None
````

#### [164. 最大间距](https://leetcode-cn.com/problems/maximum-gap/) - 桶排序

>   2020/11/25

解题思路 构造包含所有字符串的桶 解得字符串的数量并遍历惊醒排序拼接

??? "我的解题"

````python
class Solution:
    def maximumGap(self, nums: List[int]) -> int:
        if len(nums) <= 1:
            return 0
        nums.sort()
        index=0
        current = 0
        current_diff = 0
        global max
        max = 0
        for i in nums:
          pre = current
          pre_diff = current_diff
          current = i
      
          if index == 0:
              index += 1
              continue
          current_diff = current - pre
          if current_diff > max:
              max = current_diff
        return max
````

??? + "另类解题思路"

```python
class Solution:
    def maximumGap(self, nums: List[int]) -> int:
        t = sorted(nums)
        return max(y-x for x, y in zip(t, t[1:])) if len(nums) >= 2 else 0
```



````python
class Meta:

    def add(self, name):
        meta_path = os.path.join(META_SERVICE_DIR, name)
        os.mkdir(os.path.join(META_SERVICE_DIR, name))
        with open(file=os.path.join(meta_path, "ufs_chunk.cfg"), mode="w") as f:
            config = DEFAULT_CONFIG
            config["-meta_server_host="] = "127.0.0.1"
            config["-standalone_vip"] = "127.0.0.1"
            config["-cluster_uuid"] = "30856409-35d9-4da0-a020-9adb7dec78f7"
            for key, value in config:
                f.write(key + "=" + value + "\n")
````