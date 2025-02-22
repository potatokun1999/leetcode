
Step 1

# Intuition
<!-- Describe your first thoughts on how to solve this problem. -->

目的: 2^10 を求めたい。 
2^10 は、 2^5 * 2^5 である。このように分解することで効率的に求められそう。

# Approach
<!-- Describe your approach to solving the problem. -->
## 二分木として考える

2^10 = 2^5 * 2^5 の分解は、 
二分木として見たとき、親ノード (2^10) の結果は、左の子 (2^5) の結果と、右の子 (2^5) の結果の掛け合わせで、表現できる。
分割統治の左右の子の結果が同じ場合と捉えることができる。

## 二分木の構成要素

二分木としたとき、左右の子について、再帰・トラバースをする際の引数・返り値について考える。

引数
- x :子ノードも同じ x を使うため、不変の値。
- n :子ノードの n は、親ノードの n を 2 で整数除算した値.

返り値
- 「子ノードを頂点とした部分木の、かけ算の結果の値」の二乗の値

## 考慮する点: 再帰の終了条件

この再帰の終了条件は、 2^10 = ... = 2^1 * 2^1 * ... * 2^1 までの分割とする。  再帰関数内で 2^0 = 1 まで分割する理由はないと考え、n = 1 で再帰は終了する。 n = 0 については、再帰関数の外でバリデーションする。 

## 考慮する点: 奇数の場合、二分木ではない

奇数の場合の答えは、2^5 = 2^2 * 2^2 * 2 のため、 子ノードが、 3 つ存在する。
この場合、2^2 の二乗に x を掛け合わせて返却する必要がある。
再帰の過程で、n が奇数の場合があるため、再帰呼び出しの前で、偶奇の場合わけをおこなう。

## 考慮する点: n < 0 の場合わけ

n < 0 で 1 / x^n となるため、再帰関数外で、場合分けして、最終的な結果を返却する。

# Complexity
- Time complexity: O(log n)
最大深さが log n

<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(log n)
log n 回、O(1) でメモリを消費する。
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```python3 []
class Solution:
    def myPow(self, x: float, n: int) -> float:
            
        if x == 0:
            return 0
        
        if n == 0:
            return 1

        elif n < 0:
            return 1 / self.dfs(x, -n)
            
        else: 
            return self.dfs(x, n)
        


    def dfs(self, x, n):
        if n == 1:
            return x
        
        half_value = self.dfs(x, n // 2) 

        if n % 2 == 1:
            return half_value * half_value * x
        
        else:
            return half_value * half_value
    


```