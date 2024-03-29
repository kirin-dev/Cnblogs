# 2299. Strong Password Checker II

<https://leetcode.cn/problems/strong-password-checker-ii/>

> 简单模拟

```Python
class Solution:
    def strongPasswordCheckerII(self, password: str) -> bool:
        if len(password) < 8:
            return False
        
        specials = set("!@#$%^&*()-+")
        hasLower = hasUpper = hasDigit = hasSpecial = False

        for i, ch in enumerate(password):
            if i != len(password) - 1 and password[i] == password[i + 1]:
                return False

            if ch.islower():
                hasLower = True
            elif ch.isupper():
                hasUpper = True
            elif ch.isdigit():
                hasDigit = True
            elif ch in specials:
                hasSpecial = True

        return hasLower and hasUpper and hasDigit and hasSpecial
```

官方题解：<https://leetcode.cn/problems/strong-password-checker-ii/solution/qiang-mi-ma-jian-yan-qi-ii-by-leetcode-s-p7ru/>
