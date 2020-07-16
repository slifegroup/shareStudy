## python学习日常问题

### 使用 Flask 框架 + Pycharm 开启 debug 模式

![image-20200401142900255](https://img.mupaie.com/image-20200401142900255.png)

* 设置 FLASK_ENV= development
* 勾选 FLASK_DEBUG 

![image-20200401143208990](https://img.mupaie.com/image-20200401143208990.png)

> 如果不设置 FLASK_ENV 默认是生产环境

## 使用 isinstance 判断类型

```
a = 111
isinstance(a, int)
True
```

## python 中如何输入下标

使用 `enumerate`

```python
for i, j in enumerate([1, 2, 3, 4, 6]):
	print(str(i) + "-" + str(j))
```

### 关于参数 `**kwargs`， `*args`

* `**kwargs` 接受关键字，变长参数列表
* `*args ` 接受可变数量的参数，非关键字，可变长度的参数列表

### 正则表达式中\s匹配任何空白字符，包括空格、制表符、换页符等等, 等价于[ \f\n\r\t\v]

- \f -> 匹配一个换页
- \n -> 匹配一个换行符
- \r -> 匹配一个回车符
- \t -> 匹配一个制表符
- \v -> 匹配一个垂直制表符