# Find the missing term in an Arithmetic Progression

## ���� ����

� ������ ���� ��, �� �������� ���� ���� 1���� ��󳽴�.

Ex)

 * ```
   find_missing([1, 3, 5, 9, 11]) == 7
   ```

   

## ���� Ǯ�� ���

```python
import unittest


def find_missing(sequence):
    def get_last_by_diff(diff, last_value):
        if diff < 0:
            last_value -= 1
        else:
            last_value += 1
        return last_value

    first, last = sequence[0], sequence[-1]
    diff = (last - first) // len(sequence)

    last = get_last_by_diff(diff, last)
    return sum(range(first, last, diff)) - sum(sequence)

```

* ���� ���� ��, ������ �ʴ� ������ ������ �� ���� ù��° ���� �� ������ ���̿���.
* �� ������ �̿��ؼ� `diff` �� ���ϰ�, ���� `������ ��` - `�־��� ������ ��` �� �ϸ�, ���� ���� 1���� ���� ���̶�� �����ϰ� ������ Ǯ����.



## �ٸ� ����� Ǯ�� ���

### ���� ����ϴٰ� �����ϴ� Ǯ��

```python
def find_missing(sequence):
    s = set(sequence)
    m1, m2 = min(s), max(s)
    for i in range(m1, m2 + 1, (m2 - m1) // len(s)):
        if i not in s:
            return i
```

* `diff` �� ���Ͽ���, �ϼ��� ������ ���ϰ�, ����� üũ�� ���� ���� return ���ش�.
* ���δ� Ǯ�� ���ε�, �ſ� ����ϰ� �������� ���� �ڵ��� �����Ѵ�.

### �׿� �ٸ� Ǯ�� 1

```python
def find_missing(sequence):
    interval = (sequence[-1] - sequence[0])/len(sequence)
    for previous, item in enumerate(sequence[1:]):
        if item - sequence[previous] != interval:
            return item - interval
```

* ���� �� ������ ����Ʈ�� ù���� ������ ������ diff�� ���ϴ� ���̴�.
* �� ���������� �ٷ� ������ ������ ������ ���Ͽ���, interval���� �ٸ��� �ٷ� �����Ͽ���.
* �� �ڵ� ���� �ſ� ����ϰ� ������ Ǫ�µ� ����ȭ�� Ǯ�̶�� �����Ѵ�.

### �׿� �ٸ� Ǯ�� 2

```python
def find_missing(sequence):
    t = sequence
    return (t[0] + t[-1]) * (len(t) + 1) / 2 - sum(t)
```

* ������ ��ü���� ���ϴ� ���� Ȱ���Ͽ���.

  * ```
    (first + last) * len(n) / 2
    ```

* ������ �ʾҴٰ� �����ϰ� `(len(t) + 1)` �� ����ϰ�, `sum(t)` �� ����, �ſ� �����ϰ� ���� ���� �� �ִ�.



## ��� ��

* ������ �������� `diff` ���ϴ� ���� Ư�� ���� �� ��� ������ �� �� ���߾��µ�, ù���� ������ ���� Ȱ���ϴ� ����� �˰� �Ǿ���.

  * ```python
    # ������ diff ���ϱ�
    diff = (last - first) // len(sequence)
    ```

* ���� ��ü�� �� ���ϱ�

  * ```python
    # ���� ��ü�� ��
    all_sum = (first + last) * len(n) / 2
    ```

    

## �ݼ��� ��

* ������ ���õ� ������ �������� ������ Ǭ �� ������, ������ Ǯ���� ��Ŀ� ���� � ������ ã�� ����ä �� ������ Ǯ �� ó������ �ٽ� Ǯ����.
* �׷����ؼ�, ���� Ǫ�� �ð��� �þ�� diff�� ���ϸ� ���� Ǯ �� �ִ� �������µ� diff�� ���ϴ� �� ��ü ���� ����� ������ ���Ͽ��� �ð��� ���� �ɷȴ�.
* ���� ���� ���ϵ��� �̸� �˾Ҵٸ� ������ ������ ������ Ǯ���� �� �־��� �� ����.



## Action Item

* ��Ű�� ���� ��� ������ ����ϰ�, �ֱ������� Ȯ���Ѵ�.
* ������ �������� �������� ������ �� ������ ��� �͵��� Ȱ�밡������ ���ʷ� �����غ��� ������ Ǯ���.
