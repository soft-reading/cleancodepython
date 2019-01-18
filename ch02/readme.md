# Pythonic Code

每種語言有它特定的寫法的方言或慣用語法(Idiom), Idiom 是為了達到特定目標所制定的寫法，大家通常會這樣寫，但他並不是一種 `Design Pattern` ，語言中慣用的語法來解決特定的方式，而在 Python 我們稱它為 Pythonic :rolling_on_the_floor_laughing:

因為寫 Pythonic 的 Code 會讓程式更簡潔與容易解讀，所以要先來學寫 Pythonic Way，並且藉由他寫出相同模式的代碼

這章節主要會看這幾個項目

* To understand indices and slices, and correctly implement objects that can be indexed
* To implement sequences and other iterables
* To learn good use cases for context managers
* To implement more idiomatic code through magic methods
* To avoid common mistakes in Python that lead to undesired side-effects

## Indexes and slices

**使用 negative number 可以從 Array 後面來取值**

```py
my_numbers = (4, 5, 3, 9)
my_numbers[-1]
9
my_numbers[-3]
5
```

**slice是取 sub-array 的方法**

```
>>> my_numbers = (1, 1, 2, 3, 5, 8, 13, 21)
>>> my_numbers[2:5] # 拿 2,3,4
(2, 3, 5)
>>> my_numbers[:3] # 取到 index 3 之前的 sub-array
(1, 1, 2)
>>> my_numbers[3:] # 從 index 3 之後的 sub-array
(3, 5, 8, 13, 21)

>>> my_numbers[::]
(1, 1, 2, 3, 5, 8, 13, 21)

>>> my_numbers[1:7:2] # 1 - 7 隔著 2 跳
(1, 3, 8)
```

有 Slice 的概念之後可以使用 built-in slice function 來傳遞要 slice 的部分

像上面的例子可以這樣船

```
>>> interval = slice(1, 7, 2)
>>> my_numbers[interval] # 傳進來
(1, 3, 8)
>>> interval = slice(None, 3)
>>> my_numbers[interval] == my_numbers[:3]
True
```

:bulb: 儘量使用 `Slice` 的方式來做類似的事情，相反的能不用 for-loop iterate Array 就不要用