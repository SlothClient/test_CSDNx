在 Python 中，排列数和组合数的计算可以通过数学公式进行实现，或者使用 Python 内置的库来简化计算。

### 1. **排列数（Permutation）**
排列数表示从 `n` 个不同的元素中选择 `r` 个元素进行排列的方式数，计算公式为：
\[
P(n, r) = \frac{n!}{(n - r)!}
\]
其中 `n!` 表示 `n` 的阶乘。

### 2. **组合数（Combination）**
组合数表示从 `n` 个不同的元素中选择 `r` 个元素进行组合的方式数，计算公式为：
\[
C(n, r) = \frac{n!}{r!(n - r)!}
\]
其中 `r!` 表示 `r` 的阶乘。

### 3. **使用 `math` 库来计算**
Python 的标准库 `math` 提供了直接计算排列数和组合数的函数，分别是 `math.perm()` 和 `math.comb()`，你可以直接使用它们来进行计算。

#### 示例代码：
```python
import math

# 计算排列数 P(n, r)
n = 5
r = 3
permutation = math.perm(n, r)
print(f"P({n}, {r}) = {permutation}")  # 输出: P(5, 3) = 60

# 计算组合数 C(n, r)
combination = math.comb(n, r)
print(f"C({n}, {r}) = {combination}")  # 输出: C(5, 3) = 10
```

#### 解释：
- `math.perm(n, r)`：计算从 `n` 个元素中选择 `r` 个元素的排列数 `P(n, r)`。
- `math.comb(n, r)`：计算从 `n` 个元素中选择 `r` 个元素的组合数 `C(n, r)`。

### 4. **手动实现排列数与组合数**
如果你希望手动实现这些公式，可以使用 `math.factorial()` 来计算阶乘。

#### 计算排列数：
```python
import math

def permutation(n, r):
    return math.factorial(n) // math.factorial(n - r)

# 示例
n = 5
r = 3
print(f"P({n}, {r}) = {permutation(n, r)}")  # 输出: P(5, 3) = 60
```

#### 计算组合数：
```python
import math

def combination(n, r):
    return math.factorial(n) // (math.factorial(r) * math.factorial(n - r))

# 示例
n = 5
r = 3
print(f"C({n}, {r}) = {combination(n, r)}")  # 输出: C(5, 3) = 10
```

### 总结：
- **排列数** `P(n, r)` 可以通过 `math.perm(n, r)` 计算，或者手动使用阶乘公式。
- **组合数** `C(n, r)` 可以通过 `math.comb(n, r)` 计算，或者手动使用组合数公式。

如果你不需要手动实现公式，建议直接使用 `math.perm()` 和 `math.comb()`，它们简洁且高效。

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/e2/e2475c69ab10946d38216485465bd9793624789d7594a7ddcc710ae7fc64d599.png)
