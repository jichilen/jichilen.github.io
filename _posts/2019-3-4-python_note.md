---
layout: post
title:  "python note"
date:   2019-3-4
desc: "something about unicode on linux and windows"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,learning,linux,unicode]
icon: icon-html
---

最近使用xhsell做和中文有关的任务，经常被编码问题弄得头疼

主要原因还是windows和linux系统里面的编码不一致导致的，其次Python2和python3对编码的支持不同也会导致各种编码问题的出现。

windows记事本默认编码为**ANSI**，ANSI即为ASCII编码，为一个字节，只用到0~127号字符。

windows默认编码为**GBK**，是在ANSI基础上演变出来的，包含两个字节，其中中文编码与Unicode的中文编码不一样。

**Unicode**编码为万国码，包含几乎世界上的所有字符，一般情况下为两个字节。

**UTF-8**为Unicode的一种实现编码，Unicode编码可以通过一定的规则进行转变。

```python
#pycharm python3
sys.path.extend(['D:\\data\\code\\python', 'D:/data/code/python'])
Python 3.5.4 |Anaconda, Inc.| (default, Oct 27 2017, 11:35:13) [MSC v.1900 64 bit (AMD64)] on win32
print(u"\u4e3d\u6615\u7279\u7f8e\u5bb9\u7f8e\u4f53\u517b\u751f")
丽昕特美容美体养生

#pycharm python2
D:\conda\python.exe D:\PyCharm\helpers\pydev\pydevconsole.py 32473 32474
Python 2.7.13 |Anaconda, Inc.| (default, Sep 19 2017, 08:25:59) [MSC v.1500 64 bit (AMD64)]
print(u"\u4e3d\u6615\u7279\u7f8e\u5bb9\u7f8e\u4f53\u517b\u751f")
丽昕特美容美体养生

#windows cmd python2
Python 2.7.13 |Anaconda, Inc.| (default, Sep 19 2017, 08:25:59) [MSC v.1500 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> print(u"\u4e3d\u6615\u7279\u7f8e\u5bb9\u7f8e\u4f53\u517b\u751f")
?鲫刻孛廊菝捞逖?

#windows cmd python3
Python 3.5.4 |Anaconda, Inc.| (default, Oct 27 2017, 11:35:13) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> print(u"\u4e3d\u6615\u7279\u7f8e\u5bb9\u7f8e\u4f53\u517b\u751f")
丽昕特美容美体养生

#xshell win2linux python3
Python 3.5.2 (default, Nov 23 2017, 16:37:01) 
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> print(u"\u4e3d\u6615\u7279\u7f8e\u5bb9\u7f8e\u4f53\u517b\u751f")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-8: ordinal not in range(128)
    
#xshell win2linux python2
Python 2.7.12 (default, Dec  4 2017, 14:50:18) 
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> print(u"\u4e3d\u6615\u7279\u7f8e\u5bb9\u7f8e\u4f53\u517b\u751f")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-8: ordinal not in range(128)
    
#ssh linux python2   == linux python2
Python 2.7.12 (default, Dec  4 2017, 14:50:18)
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> print(u"\u4e3d\u6615\u7279\u7f8e\u5bb9\u7f8e\u4f53\u517b\u751f")
丽昕特美容美体养生

#ssh linux python3   
Python 3.5.2 (default, Nov 23 2017, 16:37:01)
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> print(u"\u4e3d\u6615\u7279\u7f8e\u5bb9\u7f8e\u4f53\u517b\u751f")
丽昕特美容美体养生
```

从上面的结果可以看到

直接在**linux使用python和在windows使用pycharm**都不会出现任何问题。

系统的编码问题确实对unicode的使用产生了很大的影响，尤其是xshell这个环境会存在着unicode转ascii 然后转unicode的问题

对于这个问题，网上众说纷纭，但是找不到一个很好的解决办法

目前尝试的比较好用的方法：

```python
#windows cmd python2
Python 2.7.13 |Anaconda, Inc.| (default, Sep 19 2017, 08:25:59) [MSC v.1500 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> a="中文"
>>> a
'\xd6\xd0\xce\xc4'
>>> print a
?形?

>>> import json
>>> print(json.dumps([u'\u4e2d\u6587', ], ensure_ascii=False))
["中文"]
>>> print(json.dumps(u'\u4e2d\u6587', ensure_ascii=False))

#此外还可以用过str来跳出这个问题
>>> print(u"\u4e3d\u6615\u7279\u7f8e\u5bb9\u7f8e\u4f53\u517b\u751f")
?鲫刻孛廊菝捞逖?
>>> print(u'"\u4e3d\u6615\u7279\u7f8e\u5bb9\u7f8e\u4f53\u517b\u751f"')
"丽昕特美容美体养生"
```

