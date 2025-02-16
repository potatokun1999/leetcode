# Step 2

# Intuition
<!-- Describe your first thoughts on how to solve this problem. -->

以下の問題の条件により、二分探索と考えた。
- ソート済配列
- 挿入位置を探索する問題で O(log n) の指定


# Approach
<!-- Describe your approach to solving the problem. -->

### 隣接する 2 要素と target の関係

target の挿入する位置に隣接する要素と、target を挿入する位置は以下の関係にある。

```
1. if target = nums[i] < nums[i+1] then i 
2. if nums[i] < target <= nums[i+1] then i+1
3. if nums[i] < nums[i+1] < target  then i+1
```

t の挿入箇所と隣接する i, i + 1 の具体的な値を l, r する。l, r を求め、上記の関係性から、最終的に target が挿入される位置を返す。  
(※ 配列の先頭より target が小さい場合もあるため、1. に包含して実装する。)

### 二分探索の探索範囲の枝刈り

t と隣接する l, r を求めたい。l - 1 以下や r + 1 以上のインデックスが target と隣接する可能性がないことが確定できたら、探索範囲から除外する。
探索範囲を二分するインデックス mid と、target は以下の関係にある。以下の条件を探索範囲の枝刈りに適用する。

- `nums[mid] <= target` の場合、 target を mid - 1 以下に挿入する可能性はなくなる。
- 反対に、`nums[mid] > target`の場合、target を mid 以上に挿入する可能性はなくなる。


# Complexity
- Time complexity:
<!-- Add your time complexity here, e.g. $$O(n)$$ -->
O(log n): 二分割は、最大 log n 回のため。
- Space complexity:
<!-- Add your space complexity here, e.g. $$O(n)$$ -->
O(1): 定数の変数のみを、定義しているため

# Code
```python3 []
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums) - 1

        while l + 1 < r:
            mid = (l + r) // 2
            if nums[mid] <= target:
                l = mid
                
            else:
                r = mid - 1
        

        if target <= nums[l]: 
            return l

        elif target <= nums[r]:
            return r

        else:
            return r + 1
```

---
# Step 3

# Approach

半開区間で書いてみる。


# Code
```python3 []
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums)

        while (l < r):
            mid = (l + r) // 2

            if nums[mid] < target:
                l = mid + 1

            elif nums[mid] > target:
                r = mid

            else:
                return mid
        
        return l 
```