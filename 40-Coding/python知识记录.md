- replace()
> lc 1108

```python
address.replace(".", "[.]")
```

- sort()、 count()、get()
```python
#  lc 1394
arr: List[int]

arr.sort()
arr.count(c)

m = dict()
m.get(x, 0)
for (key, value) in m.items():  # 遍历dict

# lc 1054
barcodes.sort(key=lambda x: (-cnt[x], x))  # 优先根据-cnt[x]从大到小排序，否则根据x从小到大排序
```

- split()、startswith()
> lc 1455
```python
sentence: str
sentence.split()

sentence[start: end].startswith(searchWord)
```
- zip()、sum()、bisect_right()、bisect_left()
```python
 # lc 1450
sum(s <= queryTime <= e for s, e in zip(startTime, endTime))

bisect_right(startTime, queryTime) - bisect_left(endTime, queryTime)

# lc 1170
return [n - bisect_right(nums, f(q)) for q in queries]
```

- sorted()

```python
# lc 977
sorted(num * num for num in nums)

# lc 2418
[name for _, name in sorted(zip(heights, names), reverse=True)]

# lc 2611
a = sorted(zip(reward1, reward2), key = lambda p : p[1] - p[0])
```

- extend()
> lc 1929

```python
nums: List[int]

nums.extend(nums)
```

- str()
> lc 1295

```python
sum(1 for num in nums if len(str(num)) % 2 == 0)
```

- lcm()
> lc 2413

```ruby
lcm(n, 2)  # 返回2,n的最小公倍数
```

- join()
> lc 2578

```python
''.join(s[1::2])
```

- enumerate()、set()、pop()
> lc 1042

```python
for i, nodes in enumerate(g):
	color[i] = (set(range(1, 5)) - {color[j] for j in nodes}).pop()
```

- Counter()
```python
#  lc 2053
freq = Counter(arr)

# lc 2423
cnt = Counter(word[:i] + word[i + 1:])
set(cnt.values())  # 可以对value进行操作
```

- accumulate()
```python
# lc 2409
DAYS_SUM = list(accumulate((31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31), initial = 0))

# lc 2559
# 用来初始化前缀和数组

```

- nonlocal
> lc 104

```python
nonlocal ans
```

- map、ord
```python
# lc 1003
st = []
for c in map(ord, s):
	if c > ord('a') and (len(st) == 0 or c - st.pop() != 1):
		return False
```

- pairwise
```python
# lc 2432
for (_, t1), (i, t) in pairwise(logs):
	t -= t1
	if t > max_t or t == max_t and i < ans:
		ans, max_t = i, t
```

- dict
```python
# lc 1
hash = dict()
for i, num in enumerate(nums):
	if target - num in hash:
		return [hash[target - num], i]
```

- all 
```python
# lc 2437
for i in range(limit):
	s = f"{i:02}"  # 两个前导零
	if all(t == '?' or t == c for t, c in zip(t, s)):
		ans += 1
    return ans
```

- bin
```python
# lc 1016
all(bin(i)[2:] in s for i in range(1, n + 1))
```

- tuple
```python
# lc 1072
cnt[tuple(row)] += 1
```

- ascii_lowercase
```python
# lc 1170
def f(s: str) -> int:  # 返回字符串s中最小字符出现的次数
	cnt = Counter(s)
	return next(cnt[c] for c in ascii_lowercase if cnt[c])

for c in ascii_lowercase:  # 能输出26个小写字母
	print(c)
```

- bit_length()
> `bit_length()` 是 Python `int` 类型的一个方法。它用于返回整数的二进制表示形式所需的位数。
```python
# lc 1483

```

- isdigit()
> `isdigit()` 函数用于检查字符串是否只包含数字字符。如果字符串只包含数字字符，则返回 True；否则，返回 False。






