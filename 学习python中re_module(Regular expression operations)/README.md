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
  r = re.findall("[^a-z0-9A-Z]",s) # 表示匹配特殊字符(比如:\n和,)
  # 所以其实字符集比通配符更实用一些\
  # 字符集里面[a,p]会被识别为3个元素,但[a-p]就是a到p的意思
  print(r)
  ```
* **(3) 重复元字符**
  重复相关的4个元字符: `{} * + ?`
  * {n,m}:数量范围贪婪符,指定左边原子的数量范围,有{n},{n,}{,m}{n,m}四种写法,其中n与m必须是非负整数
  * `*` 指定左边原子出现0次或者多次,等同{0,}(它表示0到无穷的意思)
  * `+` 指定左边原子出现1次或者多次,等同{1,}(它表示1到无穷的意思)
  * `?` 指定左边原子出现0次或者1次,等同{0,1}(它表示0次到1次)  这个`?`号也可以用来取消贪婪
      * 关键点:重复默认是贪婪匹配,可以重复符右边添加?取消贪婪匹配
  ```python
  import re
  # {}:指定左边原子的可以重复的数量范围
  s ="aeeee apple ape agree amaze age animate advertise a\ne a&e a@e a6e a9e"
  r = re.findall("a.{2}e",s) #相当于"a..e"
  r = re.findall("a[a-z]{2}e",s) #相当于"a[a-z][a-z]e"
  r = re.findall("a.{3}e",s)
  r = re.findall("a.{1,3}e",s) # 优先按最大贪婪匹配3去匹配,就是先用{3}匹配,如果不成功,最后到{1}
  r = re.findall("a.{1,3}?e",s) # 消除贪婪匹配,按左边最小的{1}开始
  r = re,findall("a.{1,}?e",s) # 思考这个
  print(r)
  # *: 指定左边原子出现0次或者多次,等同{0,}(它表示0到无穷的意思)
  # +: 指定左边原子出现1次或者多次,等同{1,}(它表示1到无穷的意思)
  # ?: 指定左边原子出现0次或者1次,等同{0,1}(它表示0次到1次)
  s ="aeeee apple ape agree amaze age animate advertise a\ne a&e a@e a6e a9e"
  r = re.findall("a.*e",s) #{0,}
  r = re.findall("a.*?e",s)
  r = re.findall("a[a-z]*?e",s)
  r = re.findall("a[a-z]+?e",s)
  r = re.findall("a[a-z]?e",s)
  r = re.findall("a[a-z]??e",s)
  print(r)
  重点理解: *?
  ```
* **(4) ^和$**
  |^|叫开始边界符或开始锚点符,匹配一行的开头位置|
  |:-:|:-:|
  |s|叫做结束边界符或结束锚点符,匹配一行的结束位置|
  ```python
  import re

  path = "/aaa/bbb/blog/2021/12/xxx/yyy/zzz"
  reg = "blog/[0-9]{4}/[0-9]{1,2}"
  ret = re.findall(reg,path)
  print(ret)
  # 显然这个是有匹配结果的,但是这个正确吗?如果作为一个URL路径,这个匹配是不合理的
  path = "/aaa/bbb/blog/2021/12/xxx/yyy/zzz"
  reg = "^blog/[0-9]{4}/[0-9]{1,2}$"
  ret = re.findall(reg,path)
  print(ret)
  # ^和$就规定了你的PATH必须以b开头}结尾,就是你必须按照reg规则,不能有其他多余的,在网络路径匹配有很大作用
  ```
* **(5) 转义符\\**
  正则中的转义符和Python字符串的转义符相似,但俩者是俩个功能
  python代码会先给python解释器,再给RE处理
  * 赋予某些普通符号特殊功能
    |元字符|描述|
    |:-|:-|
    |\d|匹配一个数字原子,等价于[0-9]|
    |\D|匹配一个非数字原子,等价于[^0-9]|
    |\w|匹配一个包括下划线的单词原子,等价于[A-Za-z0-9_]|
    |\W|匹配任何非单词字符,等价于[^A-Za-z0-9]|
    |\n|匹配一个换行符|
    |\t|匹配一个制表符,tab键|
    |\s|匹配一个任何空白字符原子,不仅仅是空格,比如还有\n\t|
    |\b|匹配一个单词边界原子,也就是指单词和非单词原子符间的位置|
    |\B|匹配一个非单词边界原子,等价于[^\b]|
    ```python
    import re
    s = "The cat sat on caterpillar"
    reg = r"\bcat\b" # 只匹配完整的`cat`
    # 为什么要用r,这里的r是raw,是为了避免于python解释器冲突,你也可以用"\\bcat\\b"
    r = re.findall(reg,s)
    print(r) # 输出[`cat`]

    s = "encrypt Jsencrypt AESencrypt encrypetDATA encrypt"
    r = re.findall(r"\bencrypt\b",s)
    print(r)
    ```
  * 取消特殊功能符号以普通化
    ```python
    # 回顾在筛选网址时,pattern = r"https?://www.[a-z].com"
    # 显然这里面.是有问题的,必须给他消除歧义
    pattern = r"https?://www\.[a-z]\.com"
    # 那就用\来取消特殊功能使其变成普通字符,其他特殊功能符号也是一样
    # 再次强调r是提示PYTHON解释器不用处理里面的字符
    ```
* **(6) ()分组与优先提取**
  * ()自带分组和优先提取的功能
    ```python
    import re
    # 示例文本,包含多个域名
    text = """ 
    Visit us at user@qq.com for more info.
    Contanct support at support@qq.com
    Also, check out admin@my163.com and info@163.com.cn
    """
    # 查找所有的匹配项
    pattern = r'\b([\w.-]+)@163\.com\b'
    # 查找所有的匹配项
    matches = re.findall(pattern,text)
    # 输出结果
    print(f"{matches}")
    ```
    对于这个例子我们可以很好的学习到分组和优先提取,但是如果我只是要分组任何进行整体重复,而不是优先提取:`(?:xxxx)`
* **(7) |或**
    * 这个理解其实很简单,就是或,看例子
      ```python
      # 案例1
      import re
      # 实例文本
      text = "I like apple, banana, and orange. I also emjoy graps."
      # 正则表达式匹配'apple','banana'或'orange'
      pattern = r'apple|banana|orange'
      # 查找所有匹配项
      matches = re.findall(pattern,text)
      #输出结果
      print(f{matches})
      ``` 
      思考()和|的组合
### 2. re模块中常用函数
  re 模块提供了一组正则处理函数,使我们在字符串中搜索匹配项:
  |函数|描述|
  |:-|:-|
  |findall|按指定的正则模式查找文本中所有的正则匹配项,以列表格式返回结果|
  |search|在字符串中**任何位置**查找首个符合正则模式的匹配项,存在则返回**re.Match对象**,不存在返回None|
  |match|判定字符串**开始位置**是否匹配正则模式的规则,匹配则返回**re.Match对象**,不存在返回None
  |split|按指定的正则模式分割字符串,返回一个分割的列表|
  |sub|把字符串按指定的正则模式来查找符合正则模式的匹配项,并可以替换一个或多个匹配项成其他内容|
  |compile|compile方法用于编译正则表达式,从而生成一个正则表达式对象.这个对象可以重用,使得多个匹配操作中更高效.

  我这里懒得举例了,以后自己忘了,就累死自己
### 3. 正则练习
建议去练习CS50 week six 关于python的习题集.