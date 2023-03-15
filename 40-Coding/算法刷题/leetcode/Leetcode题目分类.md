# 数组 、贪心
- #### [1144. 递减元素使数组呈锯齿状](https://leetcode.cn/problems/decrease-elements-to-make-array-zigzag/)
	- [题解](https://leetcode.cn/problems/decrease-elements-to-make-array-zigzag/solution/mei-you-si-lu-yi-bu-bu-ti-shi-ni-si-kao-cm0h2/)
# 数组、哈希表
- #### [1. 两数之和](https://leetcode.cn/problems/two-sum/)
	- unordered_map hash.find(x), 返回x对应的下标，如果x不存在，则返回hash.end()
# 数组、哈希表、字符串
- #### [1487. 保证文件名唯一](https://leetcode.cn/problems/making-file-names-unique/)
	- [题解](https://leetcode.cn/problems/making-file-names-unique/solution/python3javacgo-yi-ti-yi-jie-ha-xi-biao-b-tv5h/)

# 数组、位运算、哈希表
- #### [982. 按位与为零的三元组](https://leetcode.cn/problems/triples-with-bitwise-and-equal-to-zero/)
	- [题解]()
![[1677916599972.png]]
```cpp
class Solution {
public:
    int countTriplets(vector<int>& nums) {
        unordered_map<int, int> cnt;
        int n = nums.size();
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < n; j ++ )
                cnt[nums[i] & nums[j]] ++ ;
                
        int res = 0;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < 1 << 16; j ++ )
                if (!(nums[i] & j))
                    res += cnt[j];
        return res;

    }
};
```

# 数组、哈希表、有序集合
- #### [2363. 合并相似的物品](https://leetcode.cn/problems/merge-similar-items/)
	- [题解](https://leetcode.cn/problems/merge-similar-items/solution/python3javacgorust-yi-ti-yi-jie-ha-xi-bi-r7r3/)
# 数组、排序
- #### [1491. 去掉最低工资和最高工资后的工资平均值](https://leetcode.cn/problems/average-salary-excluding-the-minimum-and-maximum-salary/)
```cpp
double maxValue = *max_element(salary.begin(), salary.end());
double minValue = *min_element(salary.begin(), salary.end());
double sum = accumulate(salary.begin(), salary.end(), - maxValue - minValue);
```
```python

```
# 数组、矩阵
- #### [2373. 矩阵中的局部最大值](https://leetcode.cn/problems/largest-local-values-in-a-matrix/)
	- [题解](https://leetcode.cn/problems/largest-local-values-in-a-matrix/solution/javapythonmei-ju-mo-ni-dan-diao-dui-lie-fm0pn/)
```cpp
// 创建一个大小为(n-2)*(n-2)大小的数组，每个位置初始化为0
vector<vector<int>> res(n - 2, vector<int>(n - 2, 0));

// 枚举每个3x3矩阵的起点`(i, j), 0 <= i < n - 2, 0 <= j < n - 2`，去搜索局部的3x3矩阵最大值。
for (int i = 0; i < n - 2; i++) {
            for (int j = 0; j < n - 2; j++) {
                for (int x = i; x < i + 3; x++) {
                    for (int y = j; y < j + 3; y++) {
                        res[i][j] = max(res[i][j], grid[x][y]);
                    }
                }
            }
        }
```
```python
# Case1
ans = [[0] * (n - 2) for _ in range(n - 2)]  # 创建(n-2)*(n-2)的数组
for i in range(n - 2):
	for j in range(n - 2):
		res[i][j] = max(grid[x][y] for x in range(i, i + 3) for y in range(j, j + 3))

# Case2
res = []
	for i in range(n - 2):
		ans = []
		for j in range(n - 2):
			# 枚举每个3x3矩阵的起点，搜索局部的3x3矩阵最大值
			max_value = 0
			for a in range(i, i + 3):
				for b in range(j, j + 3):
					max_value = max(max_value, grid[a][b])
			ans.append(max_value)   # 添加当前i行的每一列结果
		res.append(ans) # 添加一行
# 先将结果存入一个[], 再将[]存入另一个[]，构成二维
```
# 数组、双指针、排序
- #### [88. 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/)
	- [题解]()
# 数学
- #### [1523. 在区间范围内统计奇数数目](https://leetcode.cn/problems/count-odd-numbers-in-an-interval-range/)
	- [题解](https://leetcode.cn/problems/count-odd-numbers-in-an-interval-range/solution/zai-qu-jian-fan-wei-nei-tong-ji-qi-shu-shu-mu-by-l/)
	- ![[1677571169463.png]]
# 动态规划
- #### [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)
![[1677588420260.png]]
# 位运算、分治
- #### [191. 位1的个数](https://leetcode.cn/problems/number-of-1-bits/)
	- [题解](https://leetcode.cn/problems/number-of-1-bits/solution/fu-xue-ming-zhu-xiang-jie-wei-yun-suan-f-ci7i/)
	- n & (n−1)，其运算结果恰为把 n 的二进制位中的最低位的 1变为 0 之后的结果


#### [2379. 得到 K 个黑块的最少涂色次数](https://leetcode.cn/problems/minimum-recolors-to-get-k-consecutive-black-blocks/)
##### 思路
- 如果blocks.size() == k, 记录‘W’的个数即可
- 如果blocks.size() > k, 暴力枚举每个长度为k的子串中'W'的个数。
```cpp
class Solution {
public:
    int minimumRecolors(string blocks, int k) {
        int res = k;
        if (k == blocks.length()){
            int cnt = 0;
            for (int i = 0; i < k; i ++ )
                if (blocks[i] == 'W')
                    cnt ++ ;
            res = min(res, cnt);
        }
        else if (blocks.length() > k){
            for (int i = 0; i <= blocks.length() - k; i ++ ){
                int cnt = 0;
                for (int j = i; j < i + k; j ++ ){
                    if (blocks[j] == 'W')
                        cnt ++ ;
                }
                res = min(res, cnt);
            }
        } 

        return res;
        
    }
};
```
- [题解](https://leetcode.cn/problems/minimum-recolors-to-get-k-consecutive-black-blocks/solution/python3javacgotypescript-yi-ti-yi-jie-hu-dmdz/)
