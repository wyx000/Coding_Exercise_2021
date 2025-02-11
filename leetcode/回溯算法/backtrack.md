- [回溯算法](#回溯算法)
  - [基本思想](#基本思想)
  - [套路框架](#套路框架)
  - [46. 全排列 Permutations [MEDIUM]](#46-全排列-permutations-medium)
  - [51. N皇后 I [HARD]](#51-n皇后-i-hard)
  - [52. N皇后 2 [HARD]](#52-n皇后-2-hard)
  - [x. N皇后思考](#x-n皇后思考)
  - [17. 电话号码的字母组合 [Medium]](#17-电话号码的字母组合-medium)
  - [22. 括号生成 [Medium]](#22-括号生成-medium)
  - [78. 子集 [Medium]](#78-子集-medium)
  - [79. 单词搜索 [Medium]](#79-单词搜索-medium)
  - [39. 组合数 [Medium] [ByteDance]](#39-组合数-medium-bytedance)

## 回溯算法

### 基本思想
**穷举**状态，遍历决策空间，选择合适的结果

### 套路框架

```
result = []
def backtrack (路径，选择列表)：
    if 满足条件：
        result.add（路径）
        return

    for choice  in  选择列表：
        make choice
        backtrack(路径， 选择列表)
        撤销选择
```



*****
### 46. 全排列 Permutations [MEDIUM]
- 模板的标准使用，指定path和状态集合(即选择集合)
- 需要判断当前选择的path是否有重复
- 时间复杂度 `O(N!)` 空间复杂度`O(N)` **取决与栈的深度，为N**
```
#include<algorithm>
class Solution {
public:
    vector<vector<int>> res;
    vector<vector<int>> permute(vector<int>& nums) {
        vector<int> path;
        backtrack(path, nums);
        return res;
    }
    void backtrack(vector<int> path, vector<int> nums) {
        if (path.size() == nums.size()) {
            res.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i ++) {
            if (find(path.begin(), path.end(), nums[i]) != path.end())
                continue;
            path.push_back(nums[i]);
            backtrack(path, nums);
            path.pop_back();
        }
    }
};
```

### 51. N皇后 I [HARD]

- 选择列表：cols即多少列；每次逐行遍历，每次做选择时验证选择是否符合N皇后冲突规则
  - 验证： 当前位置是否与之前的同列位置/左上线/右上线存在Q **遍历非常耗时，可以通过哈希表为每列/线设置Q标识，以提高速度**
  - 选择不冲突的填入board
  - 选择完成后继续向下遍历；然后撤销选择，即修改board数据，进行回溯
- 路径列表：即当前的表盘board
- 时间复杂度 `O(N!)` 空间复杂度`O(N)` 取决于栈的深度(N行)

```
class Solution {
public:
    vector<vector<string>> res;
    int rows = 0;
    vector<vector<string>> solveNQueens(int n) {
        vector<string> board;
        string row;
        for (int i = 0; i < n; i++) {
            row.push_back('.');
        }
        for (int i = 0; i < n; i++) {
            board.push_back(row);
        }
        backTrack(board, 0);
        return res;
    }
    void backTrack(vector<string>& board, int row) {
        
        if (row == board.size()) {
            res.push_back(board);
            return; 
        }
        int cols = board[row].size();
        for (int col = 0; col < cols; col++) {
            // make choice ; 
            // 判断当前是否能放
            if (!isValid(row,col,board)) {
                continue;
            }
            // 添加到path中
            board[row][col] = 'Q';
            backTrack(board, row + 1);
            // 撤销选择
            board[row][col] = '.';
        }
    }
    bool isValid(int row, int col,vector<string> board) {
        int cols = board[row].size();
        // 同列上是否有Q
        for (int i = 0; i < row; i++) {
            if (board[i][col] == 'Q')
                return false;
        }
        // 同行不用考虑
        // 右上方是否有Q： 斜线方向
        for (int i = row - 1, j = col + 1; i >= 0 && j < cols; i-- , j++) {
            if (board[i][j] == 'Q')
                return false;
        }
        // 左上方是否有Q： 斜线方向
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i-- , j--) {
            if (board[i][j] == 'Q')
                return false;
        }
        return true;
    }
};
```
- **优化：** 基于set进行验证优化,设置三个set分别对应各列/线中有Q的索引
```
class Solution {
public:
    vector<vector<string>> res;
    unordered_set<int> setCols;
    unordered_set<int> setDiag1;
    unordered_set<int> setDiag2;

    int rows = 0;
    vector<vector<string>> solveNQueens(int n) {
        vector<string> board;
        string row;
        for (int i = 0; i < n; i++) {
            row.push_back('.');
        }
        for (int i = 0; i < n; i++) {
            board.push_back(row);
        }
        backTrack(board, 0);
        return res;
    }
    void backTrack(vector<string>& board, int row) {
        
        if (row == board.size()) {
            res.push_back(board);
            return; 
        }
        int cols = board[row].size();
        for (int col = 0; col < cols; col++) {
            // make choice ; 
            // 判断当前是否能放
            // 同列冲突
            if (setCols.find(col) != setCols.end()) continue;
            if (setDiag1.find(row - col) != setDiag1.end()) continue;
            if (setDiag2.find(row + col) != setDiag2.end()) continue;

            // 添加到path中
            board[row][col] = 'Q';
            setCols.insert(col);
            setDiag1.insert(row - col);
            setDiag2.insert(row + col);
            backTrack(board, row + 1);
            // 撤销选择
            board[row][col] = '.';
            setCols.erase(col);
            setDiag1.erase(row - col);
            setDiag2.erase(row + col);
        }
    }
};
```
### 52. N皇后 2 [HARD]
- 只需要求出答案数量
- 省去存储表盘board的过程，只需要保留哈希表对结果进行验证

```
class Solution {
public:
    int totalNQueens(int n) {
        unordered_set<int> setCols;
        unordered_set<int> diag1;
        unordered_set<int> diag2;
        return backTrack(0, n, setCols, diag1, diag2);
    }
    int backTrack(int row, int n, unordered_set<int>& setCols, unordered_set<int>& diag1, unordered_set<int>& diag2) {
        if (n == row) {
            return 1;
        }
        int count = 0;
        for (int col = 0; col < n; col++) {
            if (setCols.find(col) != setCols.end()) continue;
            if (diag1.find(row + col) != diag1.end()) continue;
            if (diag2.find(row - col) != diag2.end()) continue;
            setCols.insert(col);
            diag1.insert(row + col);
            diag2.insert(row - col);
            count += backTrack(row + 1, n, setCols, diag1, diag2);
            setCols.erase(col);
            diag1.erase(row + col);
            diag2.erase(row - col);
        }
        return count;
    }
};
```
### x. N皇后思考
- 从某种程度上将，回溯法就是dp的暴力版本，穷举所有可能的情况
- 时间复杂度`O(N!)` 空间复杂度O(N)
- 当我们在遍历时如果只需要拿到一个合理结果返回即可，那么就可以修改上面的代码，当第一次遍历到底即进行返回

**调整返回逻辑**
```
   bool backTrack(vector<string>& board, int row) {
        if (row == board.size()) {
            res.push_back(board);
            return true;  
        }
        int cols = board[row].size();
        for (int col = 0; col < cols; col++) {
            // make choice ; 
            // 判断当前是否能放
            // 同列冲突
            if (setCols.find(col) != setCols.end()) continue;
            if (setDiag1.find(row - col) != setDiag1.end()) continue;
            if (setDiag2.find(row + col) != setDiag2.end()) continue;

            // 添加到path中
            board[row][col] = 'Q';
            setCols.insert(col);
            setDiag1.insert(row - col);
            setDiag2.insert(row + col);
            // 调整返回逻辑

            if (backTrack(board, row + 1))
                return true;
            // 撤销选择
            board[row][col] = '.';
            setCols.erase(col);
            setDiag1.erase(row - col);
            setDiag2.erase(row + col);
        }
        return false;
    }
};
```

### 17. 电话号码的字母组合 [Medium]
- 回溯法解题的思路总体属于中等偏上的难度
- 跟前面分析一致，需要先确定选择列表和路径列表，以及make choice的规则
- 时间复杂度： O（N!） 空间复杂度O(N)
```
class Solution {
public:
    unordered_map<char, string> numDigits;
    string digitsTarget;
    vector<string> ans;
    vector<string> letterCombinations(string digits) {
        if (digits.empty()) 
            return ans;
        numDigits.insert({'2',"abc"});
        numDigits.insert({'3',"def"});
        numDigits.insert({'4',"ghi"});
        numDigits.insert({'5',"jkl"});
        numDigits.insert({'6',"mno"});
        numDigits.insert({'7',"pqrs"});
        numDigits.insert({'8',"tuv"});
        numDigits.insert({'9',"wxyz"});
        digitsTarget = digits;
        string path = "";
        backTrack(path,0);
        return ans;
    }
    void backTrack(string& path, int num) {

        if (path.size() == digitsTarget.size()) {
            ans.push_back(path);
            return;
        }
        string digitCur = numDigits[digitsTarget[num]];
        for (auto s: digitCur) {
             path.push_back(s);
             backTrack(path, num + 1);
             path.pop_back();
        }
    }
};
```

### 22. 括号生成 [Medium]
- 针对左右括号的依赖，在进行回溯时添加依赖
  - 当`left < n`时才，才能添加左括号
  - 当`rigth < left` 时，才能添加右括号

```
class Solution {
public:
    int total = 0;
    vector<string> ans;
    vector<string> generateParenthesis(int n) {
        if (!n)
            return ans;
        total = n*2;
       // cout << total <<endl;
        string path = "";
        backTrack(path, 0, 0, 0);
        return ans;
    }
    void backTrack(string& path, int step, int right, int left) {
        if (step == total) {
            ans.push_back(path);
           // cout << "return" << endl;
            return;
        }
        //cout << step << endl;
        // 放置(的条件
        if (left < total/2) {
            path.push_back('(');
            backTrack(path, step + 1, right, left + 1);
            path.pop_back();
        }
        // 放）的条件
        if (right < left) {
            path.push_back(')');
            backTrack(path, step + 1, right + 1, left);
            path.pop_back();
        }
    }
};
```

### 78. 子集 [Medium]
- 计算数组的全部子集，要避免重复子集的出现
- 本质上就是穷举，需要添加重复子集的处理 **相当于添加剪枝操作**
  - 由于数组有序，只需要保证子集内同样有序，每次仅添加大于当前最大值的元素进入选择列表中即可 
  - 由于每层都可以算是子集，所以每次回溯都能得到一个结果，需要每次都把结果添加到结果list中


```
class Solution {
public:
    vector<vector<int>> ans;
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<int> subset;
        backTrack(subset, nums);
        return ans;
    }
    void backTrack(vector<int> subset, vector<int> nums) {
        ans.push_back(subset);
        if (subset.size() == nums.size()) {
            return;
        }
        for (int i = 0; i < nums.size(); i++) {
            if (subset.empty()) {
                subset.push_back(nums[i]);
                backTrack(subset, nums);
                subset.pop_back();
            }
            else {
                if (nums[i] > subset.back()) {
                    subset.push_back(nums[i]);
                    backTrack(subset, nums);
                    subset.pop_back();
                }
            }

        }
    }
};
```

### 79. 单词搜索 [Medium]
- 需要借助辅助数组避免字母的重复使用，即剪枝
- 首字母通过外层迭代循环来找到，然后进入回溯寻找当前位置下的结果
- 时间复杂度 `O(MN*3^L)` 空间复杂度 `O(min(L,MN))` L : word长度


```
class Solution {
public:
    int direct[4][2] = {{-1, 0}, {1, 0}, {0, 1}, {0, -1}};
    vector<vector<char>> visited;
    bool exist(vector<vector<char>>& board, string word) {
        if (word.empty() || board.empty()) {
            return false;
        }
        visited.assign(board.begin(), board.end());
        int row = 0;
        int col = 0;
        string cur;
        cur.push_back(word[0]);
        for (int i = 0; i < board.size(); i++) {
            for (int j = 0; j < board[0].size(); j++) {
                if (board[i][j] == word[0]) {
                    visited[i][j] = '0';
                    if (backTrack(board, cur, word, 1, i, j))
                        return true;
                    visited[i][j] = '1';
                }
            }
        }
        return false;
    }
    bool backTrack(vector<vector<char>>& board, string& cur, string& target, int index, int row, int col) {
        if (cur.size() == target.size()) {
            return true;
        }
        for (int i = 0; i < 4; i++) {
            int _row = row + direct[i][0];
            int _col = col + direct[i][1];
            if (_row >=0 &&  _row < board.size() &&
                _col >=0 &&  _col < board[0].size()) {
                    if (board[_row][_col] == target[index] && visited[_row][_col] !='0') {
                        cur.push_back(board[_row][_col]);
                        visited[_row][_col] = '0';
                        bool flag = backTrack(board, cur, target, index + 1, _row, _col);
                        if (flag)
                            return true;
                        cur.pop_back();
                        visited[_row][_col] = '1';
                    }
                }
            }
        return false;
    }
};
```

### 39. 组合数 [Medium] [ByteDance]
- 给定一个**无重复元素的数组** candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合
- 所有数字（包括 target）都是**正整数。 解集不能包含重复的组合**。
- 解题关键：
  - 循环结束的条件： **由于都是正整数，所以当curSum值大于target时必然要结束**
  - 如何避免重复查找： **每次仅寻找大于等于当前组合数的候选值**
  - 时间复杂度 O(N!) 空间复杂度O(N) 
```
#include<algorithm>
class Solution {
public:
    vector<vector<int>> res;
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        if (candidates.empty())
            return res;
        vector<int> path;
        backTrack(candidates, path, target, 0);
        return res;
    }
    void backTrack(vector<int>& candidates, vector<int>& path, int target, int curSum) {
        if (curSum > target) {
            return;
        }
        if (curSum == target) {
            res.push_back(path);
            return;
        }
        for (int i = 0; i < candidates.size(); i++) {
            if (!path.empty()) {
                // 去重处理
                if (path.back() > candidates[i])
                    continue;
            }
            path.push_back(candidates[i]);
            backTrack(candidates, path, target, curSum + candidates[i]);
            path.pop_back();
        }
        return;
    }
};
```