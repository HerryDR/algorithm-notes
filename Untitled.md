### **77. 组合 — 笔记**

#### **问题描述**：

给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。你可以按任何顺序返回答案。

#### **思路**：

本题要求返回所有从 `[1, n]` 中选取 `k` 个数的组合，可以使用回溯算法（backtracking）来生成所有可能的组合。

#### **回溯算法概述**：

- **回溯算法**的核心思想是**递归地尝试每个可能的选择，并在发现选择无效时返回（即回溯）**。
- 对于本题，我们从 `1` 到 `n` 中选择 `k` 个数，每次递归选择一个数，直到选择的数目达到了 `k`，即为一个合法组合。

#### **剪枝**：

- 在递归中，如果当前已经选择的元素数和剩余可以选择的元素数加起来小于 `k`，则不再继续递归，因为无法再选择足够的数来组成一个有效的组合。
- 这种剪枝可以显著减少不必要的递归计算，提高效率。

#### **代码解读**：

```java
class Solution {
    private List<List<Integer>> result = new ArrayList<>(); // 存放所有组合的结果
    private List<Integer> current = new ArrayList<>(); // 当前正在构建的组合

    public List<List<Integer>> combine(int n, int k) {
        backtrack(n, k, 1); // 从1开始选择
        return result;
    }

    private void backtrack(int n, int k, int start) {
        // 剪枝：如果剩下的数字无法满足选择k个数，提前返回
        if (current.size() + (n - start + 1) < k) {
            return;
        }

        // 如果已经选择了k个数，添加当前组合到结果
        if (current.size() == k) {
            result.add(new ArrayList<>(current)); // 创建当前组合的副本
            return;
        }

        // 遍历从start到n的数字
        for (int i = start; i <= n; i++) {
            current.add(i); // 选择当前数字
            backtrack(n, k, i + 1); // 递归，选择下一个数字
            current.remove(current.size() - 1); // 回溯，撤销选择
        }
    }
}
```

#### **关键步骤分析**：

1. **`backtrack(n, k, start)`**：
   - `n`：范围的上限，表示从 `[1, n]` 中选数。
   - `k`：选择的数目。
   - `start`：当前递归的起始数字，确保每次递归时不重复选择已经选过的数字。
2. **剪枝**：
   - **`if (current.size() + (n - start + 1) < k)`**：
      剪枝条件。如果当前组合的大小 `current.size()` 加上从 `start` 到 `n` 的剩余元素数量 `n - start + 1` 小于 `k`，则不可能组成有效的组合，直接返回，避免不必要的递归。
3. **递归**：
   - 每次递归，尝试将当前数字 `i` 添加到 `current` 中，然后递归调用，进入下一个数字的选择。
   - 如果 `current` 的大小达到了 `k`，就将其添加到结果列表 `result` 中。
   - 递归结束后，通过 `current.remove(current.size() - 1)` 回溯，撤销最后一次选择。
4. **返回结果**：
   - 在 `combine` 方法中，返回最终存储所有合法组合的 `result`。

#### **时间复杂度**：

- 最坏情况下，我们需要生成所有的 `C(n, k)` 种组合，其中 `C(n, k)` 为从 `n` 个数中选 `k` 个数的组合数。
- 因此，时间复杂度为 O(C(n, k))。

#### **空间复杂度**：

- 由于需要存储所有的组合结果，空间复杂度也是 O(C(n, k))。

#### **总结**：

1. 使用回溯算法来递归地生成所有可能的组合。
2. 在递归过程中，通过剪枝提前终止不可能的分支，提高了效率。
3. 时间复杂度和空间复杂度均与组合的数量 `C(n, k)` 成正比。