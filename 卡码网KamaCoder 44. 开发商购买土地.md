## 💻 题目描述（卡码网KamaCoder）

给定一个整数数组 Array，请计算该数组在每个指定区间内元素的总和。在一个城市区域内，被划分成了n * m个连续的区块，每个区块都拥有不同的权值，代表着其土地价值。目前，有两家开发公司，A 公司和 B 公司，希望购买这个城市区域的土地。 

现在，需要将这个城市区域的所有区块分配给 A 公司和 B 公司。

然而，由于城市规划的限制，只允许将区域按横向或纵向划分成两个子区域，而且每个子区域都必须包含一个或多个区块。 为了确保公平竞争，你需要找到一种分配方式，使得 A 公司和 B 公司各自的子区域内的土地总价值之差最小。 

注意：区块不可再分。

### 📥 输入格式

- 第一行输入两个正整数，代表 n 和 m。 
- 接下来的 n 行，每行输出 m 个正整数。

### 📤 输出格式

- 请输出一个整数，代表两个子区域内土地总价值之间的最小差距。
---

## 🧠 示例

**输入：**
```
3 3
1 2 3
2 1 3
1 2 3
```

**输出：**
```
0
```

**提示信息**
```
如果将区域按照如下方式划分：

1 2 | 3
2 1 | 3
1 2 | 3 

两个子区域内土地总价值之间的最小差距可以达到 0。

数据范围：
1 <= n, m <= 100；
n 和 m 不同时为 1。
```

---

## 🧾 解法：Python（使用前缀和 + stdin）

```python
import sys

lines = sys.stdin.read().splitlines()
grid = [[int(x) for x in lines[i].split()] for i in range(1, len(lines))]

prefix_sum = []
sum_all = 0
for row in grid:
    sum = 0
    row_prefix_sum = []
    for num in row:
        sum += num
        row_prefix_sum.append(sum)
    prefix_sum.append(row_prefix_sum)
    sum_all += row_prefix_sum[len(row_prefix_sum)-1]

sum_temp = 0
min_row = float('inf')
for row in prefix_sum:
    sum_temp += row[len(row)-1]
    if min_row > abs(2*sum_temp - sum_all):
        min_row = abs(2*sum_temp - sum_all)


min_column = float('inf')
for i in range(0, len(prefix_sum[0])):
    sum_temp = 0 
    for row in prefix_sum:
        sum_temp += row[i]
    if min_column > abs(2*sum_temp - sum_all):
        min_column = abs(2*sum_temp - sum_all)

print(min(min_column, min_row))

```

---

### 💡 说明

- 这一题的前缀和思想不是像我如上写的解法一样记录2D数组中每个位置在横向子数组中的前缀和结果，因为这样做，列向还是要loop一遍2D前缀和数组找切法，没有优化到，和没有存结果loop一遍原grid是一样的。但是如果将每列和，每行和存进两个普通一维数组再找切法，就会在包含前缀和思想的基础上为下一步找切法提供帮助，然而帮助并不大，因为找切法也只是需要loop一遍，并不需要多次计算和，不包含重复计算，所以直接算找切法和先算好存起来再找切法区别不大，前缀和必要性不大。

- 记录一下先将每列和，每行和存进两个普通一维数组再找切法的解法：

```python
# 统计横向
horizontal = [0] * n
for i in range(n):
    for j in range(m):
        horizontal[i] += vec[i][j]

# 统计纵向
vertical = [0] * m
for j in range(m):
    for i in range(n):
        vertical[j] += vec[i][j]

result = float('inf')
horizontalCut = 0
for i in range(n):
    horizontalCut += horizontal[i]
    result = min(result, abs(sum - 2 * horizontalCut))

verticalCut = 0
for j in range(m):
    verticalCut += vertical[j]
    result = min(result, abs(sum - 2 * verticalCut))

print(result)
```

复杂度：

一次遍历构造矩阵 + 累加总和：O(n * m)

统计每行总和：O(n * m)（再扫一遍所有元素）

统计每列总和：O(n * m)（再扫一遍所有元素）

切割计算：额外用O(n + m)的空间和时间保存结果并loop保存的结果

✅ 总复杂度（大 O 表示的是 渐进上界，取增长最快的一项）：O(n * m)

- 记录一下直接切边切边算的解法：

```python
# 横向切法
min_result = float('inf')
sum_temp = 0
for row in grid:
    sum_temp += sum(row)
    min_result = min(min_result, abs(2 * sum_temp - sum_all))

# 竖向切法
col_sums = [sum(grid[i][j] for i in range(n)) for j in range(m)]
sum_temp = 0
for c in col_sums:
    sum_temp += c
    min_result = min(min_result, abs(2 * sum_temp - sum_all))

print(min_result)

```

复杂度：

一次遍历构造矩阵 + 累加总和：O(n * m)

直接在遍历中做横切：O(n * m)

再次遍历矩阵做竖切：O(n * m)

✅ 总复杂度：O(n * m)
