# Introduction, Code Formattin1g, and Tools

## The meaning of clean code

* Clean Code 無法被測量
* 你以為程式語言跟電腦溝通就好了嗎？其實真正重要的語言是開發者互相溝通與傳遞訊想法，進而將其轉換成重要的程式語言
* 所以真正的 Clean Code 是開發者產出的 Code 是不是有辦法被其他開發者
  * 解讀
  * 維護

## The importance of having clean code

> Most of them revolve around the ideas of maintainability, reducing technical debt, working effectively with agile development, and managing a successful project.

* 在 Agile Development 或者 CD [continuous delivery] 中，能夠在可預測的時間推出可上線的產品， clean code 是必須的
  * 如果想要有穩定和可預測的產出， 代碼就必須要可以被好理解與好維護
* 技術債的產生可能是錯誤的決定或者錯誤的妥協，想想以後寫 code 要走捷徑？還是找尋較正確的解答
  * 債是要還的 ![lannister](http://www.quickmeme.com/img/4e/4ec7a7256a65176c0a8aaf59afba38f27999790007b666544f2747b63c70a292.jpg)

> Every time the team cannot deliver something on time and has to stop to fix and refactor the code is paying the price of technical debt.

## The role of code formatting in clean code

> Clean code is something else that goes way beyond coding standards, formatting, linting tools, and other checks regarding the layout of the code. Clean code is about achieving quality software and building a system that is robust, maintainable, and avoiding technical debt.

* 作者強調 Formatting 不是 Clean Code ， Clean Code 是比 formatting 還要更深遠的

我的解讀是

* 好結構的 code 不一定是 clean code
* 沒有結構的 code 絕對不是 clean code

### 所以秉承著 Good Coding Style 吧！

何為？

* Consistency => Easy to Read and Follow
* 容易上手與習慣同樣的寫法

建議使用 PEP-8 Standard

* Grepability

會讓 code 好被搜尋，書上提供的例子

```bash
$ grep -nr "location=" .
./core.py:13: location=current_location,
```

```bash
$ grep -nr "location =" .
./core.py:10: current_location = get_location()
```

有看出什麼差別嗎?

> PEP-8 establishes the convention that, when passing arguments by keyword to a function, we don't use spaces, but we do when we assign variables.

* Consistency - 同上個講的
* Code Quality - 容易理解的 code ，讓你一眼就看出代碼的意義

## Docstrings and annotations

> 好的 Code 本身可以很清楚在做什麼，同時他的文件也是被做足的。Documenting 應該是要寫 Code 在做什麼而不是它怎麼做這件事

* Comment 本身不是文件，而且它是很糟糕的
  * 很容易忘記維護
  * 有爛 code 才會需要註解
  * 真的非必要才寫，書中提到用了 Third-Party Lib ，某種原因要繞過不好的 Code

Documenting 可以

* 描述 DataType
* 給予例子
* annotate variables

而最重要的是可以使用 static checker 工具來做自動檢查

* [Mypy](http://mypy-lang.org/)

### Docstring

> A docstring is basically a literal string, placed somewhere in the code, with the intention of documenting that part of the logic

:exclamation: 他是文件不是註解！

使用 docting 當文件的好處是

* 書中提到 IPython 提供 `??` 就可以將 docstring 印出，例如 `dict.update??`
* 或者在 python 中輸入 `myfunc.__doc__`
* 可以使用像是 [sphinx](http://www.sphinx-doc.org/en/master/) 將 docstring 產出一個頁面，讓開發者溝通變得流暢

如果照著上面將文件輸入好與上線完美的文件，另一個問題就是需要好好的維護文件網站，如果有任何的更新，也要持續的提交上去  
這也是開發者將會面臨到的問題，因為文件可能會過舊  
所以如果開發者有改動到任何功能，也要有共識要將文件檔更新上去！

### Annotations

[PEP-3107](https://www.python.org/dev/peps/pep-3107/) 提出的概念，因為 Python 是弱型別，開發者不太知道參數 input 是什麼型別產出會是什麼型別，所以使用 annotations 可以提供使用者這樣的概念

書中的例子

```py
 class Point:
     def __init__(self, lat, long):
         self.lat = lat
         self.long = long
 def locate(latitude: float, longitude: float) -> Point:
     """Find an object in the map by its coordinates"""
```

上面的 funciton 可以使用 `__annotations__` 來查詢 annotations 帶給我們的好處

```py
>>> locate.__annotations__
  {'latitude': float, 'longitue': float, 'return': __main__.Point}
```

* 查到有趣的 [annotations 檢查](https://mozillazg.com/2016/01/python-function-argument-type-check-base-on-function-annotations.html)

### 有了 Annotations 還需要 Docstring 嗎？

書中提到這個 `function`

```py
def data_from_response(response: dict) -> dict:
   if response["status"] != 200:
       raise ValueError
   return {"data": response["payload"]}
```

雖然知道 input 和 output，但很難知道如果 `status` 不是 200 時會發生什麼事

所以還是需要告訴使用者這個 function 要怎麼給正確的 input 和 output 將會是什麼

書中的答案

```py
def data_from_response(response: dict) -> dict:
   """If the response is OK, return its payload.
   - response: A dict like::
   {
       "status": 200, # <int>
       "timestamp": "....", # ISO format string of the current
       date time
       "payload": { ... } # dict with the returned data
   }
   - Returns a dictionary like::
   {"data": { .. } }
   - Raises:
   - ValueError if the HTTP status is != 200
   """
   if response["status"] != 200:
       raise ValueError
   return {"data": response["payload"]}
```

## Configuring the tools for enforcing basic quality gates

書中提到一個概念覺得不錯，把他 quote 起來

> remember that code is for us, people, to understand, so only we can determine what is good or bad code. We should invest time in code reviews, thinking about what good code is, and how readable and understandable it is. When looking at the code written by a peer, you should ask such questions:

* Is this code easy to understand and follow for a fellow programmer?
* Does it speak in terms of the domain of the problem?
* Would a new person joining the team be able to understand it and work with it effectively?

讓 CI 檢查 code formatting, layout 讓真正的人來看 code 的邏輯才是根本之道

### Type hinting with Mypy

記得開 virtualenv

```bash
pip install mypy
```

> How do I type check my Python 2 code?
> You can use a comment-based function annotation syntax and use the --py2 command-line option to type check your Python 2 code. You’ll also need to install typing for Python 2 via pip install typing.

* 公司還在用 python2 的可以看這份[文件](https://mypy.readthedocs.io/en/latest/faq.html#how-do-i-type-check-my-python-2-code)
* 如何改寫可以參照這份[文件](https://www.python.org/dev/peps/pep-0484/#suggested-syntax-for-python-2-7-and-straddling-code)和[這份](https://mypy.readthedocs.io/en/latest/cheat_sheet.html)
* 使用 mypy 在 python 2.7 需要一些工具的設定，目前找到的作法此[部落格](https://www.alexkorablev.com/mypy-python-27.html)

### Checking the code with Pylint

```bash
pip install pylint
```

可以使用 `pylintrc` 來做 lint 的設定

### 自動化檢查

使用 Makefile 來做自動化，如下

```makefile
 typehint:
 mypy src/ tests/
 test:
 pytest tests/
 lint:
 pylint src/ tests/
 checklist: lint typehint test
 .PHONY: typehint test lint checklist
```

當執行

```bash
make checklist
```

就會自動去執行這些流程

書中介紹 [black](https://github.com/ambv/black) 來做 code formatting 的自動化，有興趣可以點連結
