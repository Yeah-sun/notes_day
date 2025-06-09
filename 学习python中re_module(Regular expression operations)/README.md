## Regular expression operations
---
## 正则表达式
Regular Expression,译作正则表达式或者正规表示法.意思是说:描述一段文本排列规则的表达式.

正则表达式并不是Python的一部分.而是一套独立于编程的语言,用于处理复杂的文本信息的强大的高级文本操作工具.正则表达式拥有自己独特语法以及一个独立的正则处理引擎,我们根据正则语法编好的规则(模式)以后,引擎不仅能够根据规则进行模糊文本查找,还可以进行模糊分割,替换等复杂的文本操作,能让开发者随心所欲地处理文本信息.正则引擎一般由编程语言提供操作,像Python就提供了re模块或regex模块来调用正则处理引擎.`这里重点强调模糊俩字`

正则表达式在处理文本的效率上比如系统自带的字符串操作,但功能却比系统自带要强大许多.

最早的正则表达式来源于Perl语言,后面其他的编程语言在提供正则表达式操作时基本沿用了Perl语言的正则语法,所有我们在学习python的正则以后,也可以在Java,PHP,JavaScript,SQL等编程语言中使用.

正则对字符串或文本的操作:分割,匹配,查找,替换;


### 1. 元字符
* **(1) 通配符**
  * 通配符,万能通配符和通配元字符:匹配一个除了换行符\n以外任何
  ``` python
  import re
  s = "apple ape agree age amaze animate advertise a\ne"
  r = re.findall("ape",s) # print ['ape']
  r = re.findall("a.e",s) # print ['ape','age','aze'] \n不匹配
  r = re.findall("a..e",s) # print ['agre','adve'] 发现这个匹配有"意思"
  r = re.findall("a...e",s) # print ['apple','agree']
  # 我们发现这个匹配并不是我们像要的,其实这里并没有设置边界
  # 我们可以用类如("\ba..e\b",s)来设置边界,之后再讲
  print(r)
  ```
* **(2) 字符集**
  * []字符集,匹配***一个***括号中出现的***任意***字符号
  ```python
  import re
  s ="apple ape agree amaze age animate advertise a\ne a&e a@e a6e a9e"
  r = re.findall("a.e",s) # print ['ape','age','aze','a&e','a@e','a6e','a9e']
  r = re.findall("a[pgz]e",s) # print ['ape','age','aze']=>这里体现了它只匹配了括号中的一个,注意是一个元素
  r = re.findall("a[a-z]e",s) # print ['ape','aze','age']=>这里匹配的是a到z的元素
  r = re.findall("a[^a-z]e",s) # 这种写法表明不需要a-e也就是取反的意思,这里的a-e不需要专门写括号
  r = re.findall("a[^a-z0-9]",s) # 同理:不匹配a-z和0-9,不需要括号隔开,^表示取反("[a-z0-9]",s)就是匹配a-z和0-9.
  r = re.findall("^a-z0-9A-Z",s) # 表示匹配特殊字符(比如:\n和,)
  # 所以其实字符集比通配符更实用一些\
  # 字符集里面[a,p]会被识别为3个元素,但[a-p]就是a到p的意思
  print(r)
  ```
* **(3) 重复元字符**
* **(4) ^和$**
* **(5) 转义符\\**
* **(6) ()分组与优先提取**
* **(7) |或**
### 2. re模块中常用函数
### 3. 正则练习