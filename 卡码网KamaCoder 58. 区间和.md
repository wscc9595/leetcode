## 💻 题目描述（卡码网KamaCoder）

给定一个整数数组 Array，请计算该数组在每个指定区间内元素的总和。

### 📥 输入格式

- 第一行输入为整数数组 `Array` 的长度 `n`  
- 接下来 `n` 行，每行一个整数，表示数组的元素  
- 随后的输入为需要计算总和的区间下标：a，b （b > = a），直至文件结束。

### 📤 输出格式

- 输出每个指定区间内元素的总和。
---

## 🧠 示例

**输入：**
```
5
1
2
3
4
5
0 1
1 3
```

**输出：**
```
3
9
```

**提示信息**
```
数据范围：
0 < n <= 100000
```

---

## 🧾 解法：Python（使用前缀和 + stdin）

```python
import sys

lines = sys.stdin.read().splitlines()
n = int(lines[0])
arr = [int(lines[i]) for i in range(1, n + 1)]

arr_sum = []
sum = 0
for i in range(0, len(arr)):
    sum = sum + arr[i]
    arr_sum.append(sum)

for line in lines[n + 1:]:
    l, r = map(int, line.strip().split())
    print(arr_sum[r] - arr_sum[l] + arr[l])
```

---

### 💡 说明

- 使用 `sys.stdin.read().splitlines()` 读取所有输入
- 构建前缀和数组 `arr_sum`，以实现常数时间区间查询
- 如果你使用带前导 0 的前缀和数组 prefix_sum，

```python
arr_sum = [0]
for num in arr:
    arr_sum.append(arr_sum[-1] + num)
```

那么区间 [l, r] 的和就是 prefix_sum[r+1] - prefix_sum[l]
