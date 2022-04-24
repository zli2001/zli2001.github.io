# Ubuntu20.04 误删 /usr/bin/python3


一次失败的尝试与宝贵的教训。
<!--more-->

## 不要删除 /usr/bin/python3,/usr/bin/python2,/usr/bin/python
当然，如果你已经看到这篇文章，也许已经晚了。

我的建议是如果你对Linux并不精通，**直接跳到重装的步骤。** 不要浪费时间试图解决了。
这样更节省时间。

如果你不死心，可以尝试以下方法。

但是以我的经验，只是会带来一个又一个更多的error。
## 可能的解决办法


### 参考链接：
1. [Ubuntu20.04中误删/usr/bin文件下的python，python3后的一系列问题](https://blog.csdn.net/qq_48020679/article/details/114796994)
2. [误删/usr/bin/目录下的文件如何处理](https://blog.csdn.net/weixin_44307065/article/details/109136808)


下面按照[文章1](https://blog.csdn.net/qq_48020679/article/details/114796994) 补充重装python3.8的方法。

其中文章1的代码有一些问题，设置软连接的地方。以及空格问题，可以看我的说明。
```bash
cd /usr/local
sudo mkdir python-3.8.3
```

```bash
sudo wget https://registry.npmmirror.com/-/binary/python/3.8.3/Python-3.8.3.tar.xz
sudo xz -d Python-3.8.3.tar.xz
sudo tar -xvf Python-3.8.3.tar #解压
```

--prefix=后面的不能有空格，否则会找不到文件。
```bash
cd Python-3.8.3
./configure --prefix=/usr/local/python-3.8.3
make #编译
make install
```


文中软连接的设置意义是： 右边为左边地址的快捷方式，因此正确方式如下。
```bash
sudo ln -s /usr/local/python-3.8.3/python3.8.3 /usr/bin/python3 #创建软连接
sudo ln -s /usr/bin/python3 /usr/bin/python
```
## 总结
**谨慎使用rm -rf**

