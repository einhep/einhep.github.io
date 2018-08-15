---
layout: post
title: "day3-algrithms-排列问题"
subtitle: "求解下一个排列"
data: 2018-08-15 23:37:00
author: "einhep"
header-img: "img/post-bg-2015"
tags:
    - 100-days-of-challenge
    - 算法
    - 机器学习
---

# [LeetCode] Permutation 全排列
这道题是求全排列问题，给的输入数组没有重复项，这跟之前的那道 Combinations 组合项 和类似，解法基本相同，但是不同点在于那道不同的数字顺序只算一种，是一道典型的组合题，而此题是求全排列问题，还是用递归DFS来求解。这里我们需要用到一个visited数组来标记某个数字是否访问过，然后在DFS递归函数从的循环应从头开始，而不是从level开始，这是和 Combinations 组合项 不同的地方，其余思路大体相同，代码如下：

解法一

    class Solution {
        public:
            vector<vector<int> > permute(vector<int> &num) {
                vector<vector<int> > res;
                vector<int> out;
                vector<int> visited(num.size(), 0);
                permuteDFS(num, 0, visited, out, res);
                return res;
            }
            void permuteDFS(vector<int> &num, int level, vector<int> &visited, vector<int> &out, vector<vector<int> > &res) {
                if (level == num.size()) res.push_back(out);
                else {
                    for (int i = 0; i < num.size(); ++i) {
                        if (visited[i] == 0) {
                        visited[i] = 1;
                        out.push_back(num[i]);
                        permuteDFS(num, level + 1, visited, out, res);
                        out.pop_back();
                        visited[i] = 0;
                        }
                    }
                }
            }
        }
还有一种递归的写法，更简单一些，这里是每次交换num里面的两个数字，经过递归可以生成所有的排列情况，代码如下：

解法二

    class Solution {
        public:
            vector<vector<int> > permute(vector<int> &num) {
                vector<vector<int> > res;
                permuteDFS(num, 0, res);
                return res;
            }
            void permuteDFS(vector<int> &num, int start, vector<vector<int> > &res) {
                if (start >= num.size()) res.push_back(num);
                for (int i = start; i < num.size(); ++i) {
                    swap(num[start], num[i]);
                    permuteDFS(num, start + 1, res);
                    swap(num[start], num[i]);
                }
            }
    };
    
最后再来看一种方法，这种方法是CareerCup书上的方法，也挺不错的，这道题是思想是这样的：

当n=1时，数组中只有一个数a1，其全排列只有一种，即为a1

当n=2时，数组中此时有a1a2，其全排列有两种，a1a2和a2a1，那么此时我们考虑和上面那种情况的关系，我们发现，其实就是在a1的前后两个位置分别加入了a2

当n=3时，数组中有a1a2a3，此时全排列有六种，分别为a1a2a3, a1a3a2, a2a1a3, a2a3a1, a3a1a2, 和 a3a2a1。那么根据上面的结论，实际上是在a1a2和a2a1的基础上在不同的位置上加入a3而得到的。

_ a1 _ a2 _ : a3a1a2, a1a3a2, a1a2a3

_ a2 _ a1 _ : a3a2a1, a2a3a1, a2a1a3
解法三:

    class Solution {
    public:
        vector<vector<int> > permute(vector<int> &num) {
            if (num.empty()) return vector<vector<int> >(1, vector<int>());
            vector<vector<int> > res;
            int first = num[0];
            num.erase(num.begin());
            vector<vector<int> > words = permute(num);
            for (auto &a : words) {
                for (int i = 0; i <= a.size(); ++i) {
                    a.insert(a.begin() + i, first);
                    res.push_back(a);
                    a.erase(a.begin() + i);
                }
            }
            return res;
        }
    };

# [LeetCode] Next Permutation 下一个排列
 

Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place, do not allocate extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1

 

这道题让我们求下一个排列顺序，有题目中给的例子可以看出来，如果给定数组是降序，则说明是全排列的最后一种情况，则下一个排列就是最初始情况，可以参见之前的博客 Permutations 全排列。我们再来看下面一个例子，有如下的一个数组

1　　2　　7　　4　　3　　1

下一个排列为：

1　　3　　1　　2　　4　　7

那么是如何得到的呢，我们通过观察原数组可以发现，如果从末尾往前看，数字逐渐变大，到了2时才减小的，然后我们再从后往前找第一个比2大的数字，是3，那么我们交换2和3，再把此时3后面的所有数字转置一下即可，步骤如下：

1　　2　　7　　4　　3　　1

1　　2　　7　　4　　3　　1

1　　3　　7　　4　　2　　1

1　　3　　1　　2　　4　　7

解法1:

    class Solution {
    public:
        vector<vector<int> > permute(vector<int> &num) {
            vector<vector<int> > res;
            vector<int> out;
            vector<int> visited(num.size(), 0);
            permuteDFS(num, 0, visited, out, res);
            return res;
        }
        void permuteDFS(vector<int> &num, int level, vector<int> &visited, vector<int> &out, vector<vector<int> > &res) {
            if (level == num.size()) res.push_back(out);
            else {
                for (int i = 0; i < num.size(); ++i) {
                    if (visited[i] == 0) {
                        visited[i] = 1;
                        out.push_back(num[i]);
                        permuteDFS(num, level + 1, visited, out, res);
                        out.pop_back();
                        visited[i] = 0;
                        }
                    }
                }
            }
    };

解法二：

    class Solution {
    public:
        void nextPermutation(vector<int>& nums) {int n = nums.size(), i = n - 2, j = n - 1;
            while (i >= 0 && nums[i] >= nums[i + 1]) --i;
            if (i >= 0) {
                while (nums[j] <= nums[i]) --j;
                swap(nums[i], nums[j]);
            }
            reverse(nums.begin() + i + 1, nums.end());
        }
    };
    
# [LeetCode] Permutations II 全排列之二
这道题是之前那道 Permutations 全排列的延伸，由于输入数组有可能出现重复数字，如果按照之前的算法运算，会有重复排列产生，我们要避免重复的产生，在递归函数中要判断前面一个数和当前的数是否相等，如果相等，前面的数必须已经使用了，即对应的visited中的值为1，当前的数字才能使用，否则需要跳过，这样就不会产生重复排列了，代码如下：
解法一：

    class Solution {
    public:
        vector<vector<int> > permuteUnique(vector<int> &num) {
            vector<vector<int> > res;
            vector<int> out;
            vector<int> visited(num.size(), 0);
            sort(num.begin(), num.end());
            permuteUniqueDFS(num, 0, visited, out, res);
            return res;
        }
        void permuteUniqueDFS(vector<int> &num, int level, vector<int> &visited, vector<int> &out, vector<vector<int> > &res) {
            if (level >= num.size()) res.push_back(out);
            else {
                for (int i = 0; i < num.size(); ++i) {
                    if (visited[i] == 0) {
                        if (i > 0 && num[i] == num[i - 1] && visited[i - 1] == 0) continue;
                        visited[i] = 1;
                        out.push_back(num[i]);
                        permuteUniqueDFS(num, level + 1, visited, out, res);
                        out.pop_back();
                        visited[i] = 0;
                    }
                }
            }
        }
    };

还有一种比较简便的方法，在Permutations的基础上稍加修改，我们用set来保存结果，利用其不会有重复项的特点，然后我们再递归函数中的swap的地方，判断如果i和start不相同，但是nums[i]和nums[start]相同的情况下跳过，继续下一个循环，参见代码如下：
解法二：

    class Solution {
    public:
        vector<vector<int>> permuteUnique(vector<int>& nums) {
            set<vector<int>> res;
            permute(nums, 0, res);
            return vector<vector<int>> (res.begin(), res.end());
        }
        void permute(vector<int> &nums, int start, set<vector<int>> &res) {
            if (start >= nums.size()) res.insert(nums);
            for (int i = start; i < nums.size(); ++i) {
                if (i != start && nums[i] == nums[start]) continue;
                swap(nums[i], nums[start]);
                permute(nums, start + 1, res);
                swap(nums[i], nums[start]);
            }
        }
    };

