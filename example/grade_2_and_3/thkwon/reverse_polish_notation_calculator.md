# Reverse polish notation calculator

## ���� ����

 [Reverse Polish notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation) �����̴�

* �밳�� `1 + 2 = 3` �̷��� ǥ���Ǵµ�, `1 2 + = 3 ` �̷��� ǥ���Ǵ� ���� ���Ѵ�.  



## ���� Ǯ�� ���

```python
# ���� Ǯ��

import unittest
import operator
from collections import deque

OPERATORS = {'+', '-', '*', '/'}


def calc(expr):

    if not expr:
        return 0

    expr_list = expr.split()
    if len(expr_list) == 1:
        return (int(expr_list[0])
                if isinstance(expr_list[0], int)
                else float(expr_list[0]))

    expr_num_deque = deque()
    for idx, ele in enumerate(expr_list):
        if ele in OPERATORS:
            prev = expr_num_deque.pop()
            prev_prev = expr_num_deque.pop()
            expr_num_deque.append(_get_result_by_notation(ele, prev, prev_prev))
        else:
            expr_num_deque.append(int(ele))

    return expr_num_deque[0]
```



```python
# ���� �� ������ �ڵ�

from operator import add, floordiv, mul, sub
import unittest

OPERATIONS = {"+": add, "-": sub, "*": mul, "/": floordiv}


def calc(expr):
    stack = []
    for ele in expr.split():
        if ele in OPERATIONS:
            prev = stack.pop()
            prev_prev = stack.pop()
            stack.append(OPERATIONS[ele](prev_prev, prev))
        else:
            stack.append(float(ele))

    return (stack and stack[0]) or 0
```

* stack�� �̿��Ͽ��� ������ Ǯ����.
* stack �� ����� �����̸� stack�� �ְ�, opertor�̸� ���� ���� �� 2���� ������ operator�� �°� ����ϵ��� �Ͽ� ������ Ǯ����.



```python
# �׽�Ʈ

class TestCalc(unittest.TestCase):
    def test_should_return_0_when_given_expr_is_empty_string(self):
        expr = ""
        actual = calc(expr)
        self.assertEqual(actual, 0)

    def test_calc_with_only_number_not_notation(self):
        expr = "3"
        actual = calc(expr)
        self.assertEqual(actual, 3)

    def test_calc_with_only_float_number_not_notation(self):
        expr = "3.5"
        actual = calc(expr)
        self.assertEqual(actual, 3.5)

    def test_calc_with_only_two_numbers_and_one_noation(self):
        expr = "1 3 +"
        actual = calc(expr)
        self.assertEqual(actual, 4)

    def test_calc_with_only_two_numbers_and_one_noation_is_plus(self):
        expr = "1 3 +"
        actual = calc(expr)
        self.assertEqual(actual, 4)

    def test_calc_with_only_two_numbers_and_one_noation_is_minus(self):
        expr = "1 3 -"
        actual = calc(expr)
        self.assertEqual(actual, -2)

    def test_calc_with_only_two_numbers_and_one_noation_is_multiply(self):
        expr = "1 3 *"
        actual = calc(expr)
        self.assertEqual(actual, 3)

    def test_calc_with_only_two_numbers_and_one_noation_is_division(self):
        expr = "10 2 /"
        actual = calc(expr)
        self.assertEqual(actual, 5)

    def test_calc_with_multiple_numbers_and_multiple_notations(self):
        expr = "5 1 2 + 4 * + 3 -"
        actual = calc(expr)
        self.assertEqual(actual, 14)
```





## �ٸ� ����� Ǯ�� ���

### Best Practice for me

```python
from operator import add, sub, mul, truediv

ops = {'+': add, '-': sub, '*': mul, '/': truediv}

def calc(expr):
    stack = []
    for x in expr.split():
        if x in '+-*/':
            b, a = stack.pop(), stack.pop()
            x = ops[x](a, b)
        stack.append(float(x))
    return (stack or [0]).pop()
```

* ���ó� ������ �ڵ��ε�, ���� �ۼ��� �ڵ�� ����ѵ� �ξ��� �ڵ尡 ����� ������ ���.

### Best Practice

```python
import operator

def calc(expr):
    OPERATORS = {'+': operator.add, '-': operator.sub, '*': operator.mul, '/': operator.truediv}
    stack = [0]
    for token in expr.split(" "):
        if token in OPERATORS:
            op2, op1 = stack.pop(), stack.pop()
            stack.append(OPERATORS[token](op1,op2))
        elif token:
            stack.append(float(token))
    return stack.pop()
```

* �̰� Best Practice �ε�, ��� �� ������ �κе� ���δ�
* �� �ڵ�� ���� ����ѵ�, ����� �Լ� ������ �������ٴ���, split�� �� ���� (" ")�� ���ʿ䰡 ���ٴ����� �ڵ帮�� �Ÿ��� ���δ�

### Clever

```python
operator = set(['+', '-', '*', '/'])

def calc(expr):
    stack = list()
    for c in expr.split():
        if c in operator : 
            first = stack.pop()
            second = stack.pop()
            stack.append(str(eval(second + c + first)))
        else : stack.append(c)
    return eval(stack.pop()) if stack else 0
```

* �� �ڵ嵵 ���� ����ѵ�, �ڵ帮�� ����Ʈ�� ��� �ִ�
  * stack�� list()�� ������ �ʿ䰡 ���� ����.
  * operator �� ������ ���� ��, ���� ����Ʈ�� ���� �ٽ� set�� ���� �ʿ䰡 ����, ����� ó���� �� �̿��ٸ�, `operator` �� �빮�ڷ� ���־�� �Ѵ�.
  * `eval` �� ����Ͽ��µ�, Ư���� ��찡 �ƴ϶�� �ظ��ϸ� ���� �ʴ� ���� ���� �� ����.



## ��� ��

* stack ���� ������ ������ ���ݾ� �ٸ��� �о��, stack�� ����ؾ��� �� ���� ������ ����. �� ������ �׷��߰�, ���ð��� ���� ���� �ϳ��� �� �����.



## �ݼ��� ��

* ó���� ���� ���� �� ���� stack �� ������ ���Ͽ��ٰ�, ���� �׽�Ʈ�� �ѹ� Ʋ���� ���� �� ������ stack�� ����ϴ� �� �̶�� ���ݰ� �Ǿ���. ������ ����� ������ ������ �ٷ� stack���� ������ Ǯ �� �ֵ��� ����
* ù��° ������ �� ��, operator if ���� ���� �κп��� dict �� ������ �� �ִ� �Ϳ� ���ؼ� ���ƴ�. �����ϰ� dict�� ǥ���ȴٸ�, ���������� if ���� ������� ���� �ذ��غ����� ����.



## Action Item

* stack ���� ������ ���ؼ� �ľ��ϰ� ���� ������ ������ �ٷ� �ٷ� ����
  * Postfix operator ����
  * ��ȣ ¦ ���� ���� ���


