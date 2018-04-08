## 魔法方法

### 1. 迭代器
>迭代器就是重复地做一些事情，可以简单的理解为循环，在python中实现了__iter__方法的对象是可迭代的，实现了next()方法的对象是迭代器

1. 实际要让一个迭代器工作，至少要实现\_\_iter\_\_方法和next方法。

2. 多时候使用迭代器完成的工作使用列表也可以完成，但是如果有很多值列表就会占用太多的内存，而且使用迭代器也让我们的程序更加通用、优雅、pythonic。

3. 如果一个类想被用于for ... in循环，类似list或tuple那样，就必须实现一个\_\_iter\_\_()方法，该方法返回一个迭代对象，然后，Python的for循环就会不断调用该迭代对象的next()方法拿到循环的下一个值，直到遇到StopIteration错误时退出循环。

	<font color=green>python第一步会调用\_\_iter\_\_方法，也只会调用一次，以后会执行\_\_iter\_\_返回结果的\_\_next\_\_方法，next方法中不能再有for循环，要记得抛出StopIteration异常，外层的for循环会捕获这个异常作为终止条件。</font>

	
### 2. 迭代器和yield from 结合
yield from 和yield 一样，解释器一看到yield就立刻返回一个生成器，不会向下执行。
yield from 的对象如果不是一个生成器，就应该是一个迭代器。
1. 迭代器：
s
	yield from iterator： 
	相当于：
	for i in iterator：
		yield i
<font color=green>即，yield from iterator 会先调用一次iterator的\_\_iter\_\_方法，然后依次调用iter返回对象的next方法。</font>

生成器也有next方法，也是可迭代对象。
		