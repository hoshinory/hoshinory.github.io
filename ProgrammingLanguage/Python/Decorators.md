[toc]

# 1. 前置知识

## 1.1 闭包(Closure)

### 1.1.1 闭包是什么?

> 闭包可以用来在一个函数与一组“私有”变量之间建立关联关系。在给定函数被多次调用的过程中，这些私有变量能够保持其持久性。变量的作用域仅限于包含它们的函数，因此无法从其它程序代码部分进行访问。不过，变量的生存期是可以很长，在一次函数调用期间所建立所生成的值在下次函数调用时仍然存在。正因为这一特点，闭包可以用来完成信息隐藏，并进而应用于需要状态表达的某些编程范型中[^1]

**关键词**:

- [x] 作用域
- [x] “私有”变量

假设我们要计算如下的函数表达式:
$$f(x) = (x + y)^2, x = 5$$

使用python实现一下:

```python
    def compute(x):
        def wrapper(y):
            return (x + y)**2
        return wrapper

    compute_sum_square = compute(x = 5)
    for i in range(10):
        print(compute_sum_square(i))
```

[^1]: <https://zh.wikipedia.org/zh-hans/%E9%97%AD%E5%8C%85_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)#%E8%AF%8D%E6%BA%90>
