考试要背的代码
```python
import math

# ==========================================
# 辅助函数（示例方程：x^3 - x - 1 = 0 及其导数）
# ==========================================

def f(x):
    """示例方程 f(x) = x^3 - x - 1"""
    return x**3 - x - 1

def df(x):
    """示例方程的一阶导数 f'(x) = 3x^2 - 1"""
    return 3 * x**2 - 1

def g(x):
    """不动点迭代的构造形式：x = (x + 1)^(1/3)"""
    return (x + 1)**(1/3)

# ==========================================
# 方法一：二分法 (Bisection Method)
# ==========================================

def bisection(f, a, b, tol=1e-6, max_iter=100):
    """
    二分法求根
    :param f: 目标函数
    :param a, b: 区间端点，需满足 f(a)*f(b) < 0
    :param tol: 精度控制
    :param max_iter: 最大迭代次数
    :return: 近似根
    """
    if f(a) * f(b) >= 0:
        raise ValueError("区间端点函数值必须异号！")
    
    for i in range(max_iter):
        mid = (a + b) / 2
        if f(mid) == 0 or (b - a) / 2 < tol:
            return mid
        if f(a) * f(mid) < 0:
            b = mid
        else:
            a = mid
    
    return (a + b) / 2

# ==========================================
# 方法二：不动点迭代法 (Fixed-Point Iteration)
# ==========================================

def fixed_point(g, x0, tol=1e-6, max_iter=100):
    """
    不动点迭代法求根
    :param g: 不动点迭代函数 x = g(x)
    :param x0: 初始猜测值
    :param tol: 精度控制
    :param max_iter: 最大迭代次数
    :return: 近似根
    """
    x = x0
    for i in range(max_iter):
        x_new = g(x)
        if abs(x_new - x) < tol:
            return x_new
        x = x_new
    return x

# ==========================================
# 方法三：牛顿法 (Newton's Method)
# ==========================================

def newton(f, df, x0, tol=1e-6, max_iter=100):
    """
    牛顿法求根
    :param f: 目标函数
    :param df: 目标函数的一阶导数
    :param x0: 初始猜测值
    :param tol: 精度控制
    :param max_iter: 最大迭代次数
    :return: 近似根
    """
    x = x0
    for i in range(max_iter):
        fx = f(x)
        if abs(fx) < tol:
            return x
        dfx = df(x)
        if dfx == 0:
            raise ZeroDivisionError("导数为零，牛顿法失效！")
        x_new = x - fx / dfx
        if abs(x_new - x) < tol:
            return x_new
        x = x_new
    return x

# ==========================================
# 方法四：弦截法 (Secant Method)
# ==========================================

def secant(f, x0, x1, tol=1e-6, max_iter=100):
    """
    弦截法求根（不需要导数）
    :param f: 目标函数
    :param x0, x1: 两个初始猜测值
    :param tol: 精度控制
    :param max_iter: 最大迭代次数
    :return: 近似根
    """
    for i in range(max_iter):
        f0 = f(x0)
        f1 = f(x1)
        if abs(f1) < tol:
            return x1
        if f1 - f0 == 0:
            raise ZeroDivisionError("分母为零，弦截法失效！")
        x2 = x1 - f1 * (x1 - x0) / (f1 - f0)
        if abs(x2 - x1) < tol:
            return x2
        # 更新变量（注意顺序）
        x0 = x1
        x1 = x2
    return x1

# ==========================================
# 测试运行
# ==========================================

if __name__ == "__main__":
    print("=== 数值求根方法测试 (方程: x^3 - x - 1 = 0) ===")
    print(f"精确解约等于: {1.324717957} \n")
    
    print(f"二分法结果: {bisection(f, 1, 2):.10f}")
    print(f"不动点迭代结果: {fixed_point(g, 1.5):.10f}")
    print(f"牛顿法结果: {newton(f, df, 1.5):.10f}")
    print(f"弦截法结果: {secant(f, 1, 2):.10f}")
```