---
title: 质数（Prime Numbers）
cover: /img/covers/prime-numbers.jpg
date: 2026-06-24 14:10:00
top_img: https://picsum.photos/seed/primenumbers/1920/600
tags:
  - 质数
  - 数学
  - 算法
categories:
  - Java课后作业
---

## 一、什么是质数

质数（Prime Number），又称素数，是指在大于 1 的自然数中，除了 1 和它本身以外，不能被其他自然数整除的数。

例如：
- **2、3、5、7、11、13、17、19** 都是质数。
- **4、6、8、9、10** 则不是质数（称为合数）。

## 二、质数的基本性质

1. **唯一分解定理**：任何一个大于 1 的整数，都可以唯一地表示为若干个质数的乘积（不考虑顺序）。
   例如：
   ```
   60 = 2 × 2 × 3 × 5 = 2² × 3 × 5
   ```

2. **质数有无穷多个**：古希腊数学家欧几里得通过反证法证明了质数的个数是无限的。

3. **2 是唯一的偶质数**：所有大于 2 的偶数都能被 2 整除，因此不是质数。

## 三、判断质数的简单方法

### 试除法

对于一个数 $n$，只需检查从 2 到 $\sqrt{n}$ 之间的整数是否能整除 $n$。如果都不能，则 $n$ 是质数。

**Python 示例代码**：

```python
import math

def is_prime(n):
    if n < 2:
        return False
    for i in range(2, int(math.sqrt(n)) + 1):
        if n % i == 0:
            return False
    return True

# 测试
print(is_prime(17))  # True
print(is_prime(18))  # False
```

### 埃拉托斯特尼筛法

如果需要找出一定范围内的所有质数，可以使用筛法，效率更高。

```python
def sieve_of_eratosthenes(limit):
    is_prime = [True] * (limit + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(limit ** 0.5) + 1):
        if is_prime[i]:
            for j in range(i * i, limit + 1, i):
                is_prime[j] = False
    return [i for i, prime in enumerate(is_prime) if prime]

print(sieve_of_eratosthenes(50))
# [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47]
```

## 四、质数的应用

质数在计算机科学和日常生活中有着广泛的应用：

- **密码学**：RSA 加密算法依赖于大质数分解的困难性。
- **哈希函数**：质数常用于设计哈希表的大小，以减少冲突。
- **随机数生成**：质数在一些伪随机数生成算法中扮演重要角色。
- **数论研究**：质数是数论的核心研究对象之一。

## 五、有趣的质数

- **孪生质数**：差为 2 的质数对，如 (3, 5)、(11, 13)、(17, 19)。
- **梅森质数**：形如 $2^p - 1$ 的质数，其中 $p$ 也是质数。
- **哥德巴赫猜想**：任一大于 2 的偶数都可以写成两个质数之和，至今未被完全证明。

## 六、总结

质数是数学中最基础、最神秘的概念之一。从古希腊到现代密码学，质数始终吸引着无数数学家和计算机科学家的目光。通过本次学习，我不仅回顾了质数的基本定义和判定方法，也了解了它在实际应用中的重要作用。
