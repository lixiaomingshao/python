# random妙用

python的[random](https://docs.python.org/3.5/library/random.html)模块可以随机生成密码，下面我们来尝试一下：

## 一、random.randint()

random.randint()的用法如下：

	random.randint(a, b)
		Return a random integer N such that a <= N <= b. Alias for randrange(a, b+1).


### 1、配合高阶函数reduce使用

创建一个test1.py：

	#!/usr/bin/env python3
	# -*- coding: utf-8 -*-

	import random
	from functools import reduce
	def akb():
    	return [random.randint(0,9) for av in range(6)]
	def ske(a,v):
    	return a*10+v
	if __name__=="__main__":
		for av in range(3):
    		print(reduce(ske,akb()))

运行结果如下：

	959483
	600518
	8851

纳尼，最后一个怎么和前面的不同啊。当然啦，akb()生成的列表前面如果是0，利用高阶函数reduce通过ske作用于akb()，0乘以10等于0，最后一个结果本应是008851，没有格式化，所以最终是8851。

### 2、配合格式化使用

创建一个test2.py：

	#!/usr/bin/env python3
	# -*- coding: utf-8 -*-

	import random
	def akb():
    	return [random.randint(0,9) for av in range(6)]
	if __name__=="__main__":
    	for av in range(3):
        	print("%d%d%d%d%d%d"%tuple(akb()))

运行结果如下：

	608973
	843470
	350633

## 二、random.choice()

前面的代码只是生成数字密码，如果想生成数字、大小写英文字母和特殊符号的混合密码呢？表急，我们可以用random.choice()。random.choice的用法如下：

	random.choice(seq)
    	Return a random element from the non-empty sequence seq. If seq is empty, raises IndexError.

创建一个test3.py：

	#!/usr/bin/env python3
	# -*- coding: utf-8 -*-

	import random
	import string
	def akb():
    	ske=string.ascii_letters+string.digits+"!@#$%^&*()"
    	return ''.join([random.choice(ske) for av in range(6)])
	if __name__=="__main__":
    	for av in range(3):
        	print(akb())

运行结果如下：

	^zLN8h
	g@cpJz
	0F05*V

## 三、生活中的使用

### 1、密码

有时候我们需要6位数的数字密码，有时候我们需要8位数的混合密码。好吧，创建一个password.py:

	#!/usr/bin/env python3
	# -*- coding: utf-8 -*-

	import random
	import string
	length=int(input("请输入你需要的密码位数："))
	ske=input("请输入你需要的密码形式（如需要全数字则输入1，输入其它则是数字、大小写英文字母和特殊符号的密码形式）：")
	def akb():
    	if ske=="1":
        	return ''.join([random.choice(string.digits) for av in range(length)])
    	else:
        	chrs=string.ascii_letters+string.digits+"!@#$%^&*()"
        	return ''.join([random.choice(chrs) for av in range(length)])
	if __name__=="__main__":
    	for av in range(3):
        	print(akb())

运行结果如下：

	请输入你需要的密码位数：6
	请输入你需要的密码形式（如需要全数字则输入1，输入其它则是数字、大小写英文字母和特殊符号的密码形式）：av
	c0#986
	)(9@&U
	fWeUx)

### 2、购买双色球

其实random还可以拿来购买彩票，我们以双色球来举个粟子，双色球玩法是先从1-33个红球中选出6个红球，然后再从1-16个蓝球中选出1个蓝球。而且红球不能有重复，好吧，外甥打灯笼——照旧，先创建一个lottery.py:

	#!/usr/bin/env python3
	# -*- coding: utf-8 -*-

	import random
	def akb():
		ske=[]
		while len(ske)<6:
        	nmb=random.randint(1,33)
        	hkt=0
        	if len(ske)>=1:
            	for av in range(len(ske)):
                	if nmb==ske[av]:
                    	hkt=1
        	if hkt==0:
            	ske.append(nmb)
    	return ["红球",sorted(ske)]+["蓝球",random.randint(1,16)]
	if __name__=="__main__":
    	print(akb())

运行结果如下：

	['红球', [9, 10, 14, 20, 22, 25], '蓝球', 7]

新年快乐！