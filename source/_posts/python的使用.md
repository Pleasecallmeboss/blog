---
title: python的使用
date: 2022-03-31 16:54:15
tags:
---

## conda的常用命令

https://blog.csdn.net/hejp_123/article/details/92151293



## Resource punkt not found.

https://blog.csdn.net/qq_24263553/article/details/105726751

### 导入包的问题 sys.path

直接运行 和 模块运行 的区别
python xxx.py
python -m xxx.py
这是两种加载py文件的方式:
1叫做直接运行
2把模块当作脚本来启动(注意：但是__name__的值为'main' )

直接启动是把xx.py文件，所在的目录放到了sys.path属性中。
模块启动是把你输入命令的目录（也就是当前路径），放到了sys.path属性中

作者：大富帅
链接：https://www.jianshu.com/p/04701cb81e38
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

### requests 的session的使用 重定向问题
会话也可用来为请求方法提供缺省数据。这是通过为会话对象的属性提供数据来实现的：

s = requests.Session()
s.auth = ('user', 'pass')
s.headers.update({'x-test': 'true'})

# both 'x-test' and 'x-test2' are sent
s.get('http://httpbin.org/headers', headers={'x-test2': 'true'})
任何你传递给请求方法的字典都会与已设置会话层数据合并。方法层的参数覆盖会话的参数。

不过需要注意，就算使用了会话，方法级别的参数也不会被跨请求(指两次session.get       )保持。下面的例子只会和第一个请求发送 ​cookie​ ，而非第二个：

s = requests.Session()

r = s.get('http://httpbin.org/cookies', cookies={'from-my': 'browser'})
print(r.text)
# '{"cookies": {"from-my": "browser"}}'

r = s.get('http://httpbin.org/cookies')
print(r.text)
# '{"cookies": {}}'

