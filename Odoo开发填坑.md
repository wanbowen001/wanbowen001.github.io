# Odoo开发填坑
## 0717 Wed
### 填坑05
> wanbowen
- 进程守护 **systemd** 的使用:
```
[Unit]
Description=Odoo
After=postgresql.service#绑定数据库,只要数据库启动了,主进程就不会被干掉.

[Service]
Type=simple
ExecStart=/root/odoo-dev/odoo/odoo-bin -c /root/odoo.conf#这里填入的路径是根据服务器而定,但是千万不要太长,太长就会爆status=203/EXEC,非常麻烦

[Install]
WantedBy=multi-user.target
```


## 0710 Wed

### 填坑04
> wanbowen

- 系统环境：Ubuntu 18.04
- 代码环境：Python 虚拟环境
执行 ```./odoo/odoo-bin``` 发生如下错误：
![04_1](https://images.gitee.com/uploads/images/2019/0713/185914_8c7a4b43_5136250.png "屏幕截图.png")

通过网络资料查询，可能是python版本污染的问题。重新安装虚拟环境问题未得到解决。而后通过进一步查询最终意识到是昨天犯下的一个错误。由于GitHub不同终端push内容不一致且强制push导致内容冲突致使文件内容被改动。文件内容的变动导致整个项目运行失败。
- 总结：谨慎使用Git版本管理仓库系统。


## 0708 Mon
### 填坑03
> wanbowen

当发生如下报错时：
```
Uncaught TypeError: Cannot read property 'getBoundingClientRect' of undefined （CSS样式错误）
```
意味着错误是来自于libsass库，执行如下代码即可：
```
pip3 install libsass
```

## 0703 Wed

### 填坑02
> wanbowen

执行如下启动odoo：

![02_1](https://images.gitee.com/uploads/images/2019/0713/185752_a8b2a780_5136250.png "屏幕截图.png")

报出如下错误：

![02_2](https://images.gitee.com/uploads/images/2019/0713/185727_650da308_5136250.png "屏幕截图.png")

**解决方案:**

进入如下文件进行修改：
![02_3](https://images.gitee.com/uploads/images/2019/0713/185658_1f55fa6e_5136250.png "屏幕截图.png")
将所有的“peer”修改为：“trust”，如图：

![02_4](https://images.gitee.com/uploads/images/2019/0713/185548_e500cc3f_5136250.png "屏幕截图.png")

接下来reload数据库，成功后再开启项目：

![02_5](https://images.gitee.com/uploads/images/2019/0713/185438_1c8d17ab_5136250.png "屏幕截图.png")

最终问题解决。

### 填坑01
> wanbowen

- 系统版本：阿里云·Ubuntu 18.04
- Python版本：3.6.8
- 问题描述：在安装完PostgreSQL服务+Odoo依赖 ![01_1](https://images.gitee.com/uploads/images/2019/0713/185327_7a1a3a3f_5136250.png "屏幕截图.png")
接下来从GitHub官方clone odoo项目：
```
git clone https://github.com/odoo/odoo.git -b 12.0 --depth=1 # 获取 Odoo 源码
```
接着安装requirements.txt声明的依赖文件：
```
pip3 install -r ~/odoo-dev/odoo/requirements.txt
```
发现多个报错。
没办法，根据所缺的module一个个补上安装，最终将所缺的module freeze到requirments.txt文件夹内：

```
argh==0.26.2
Babel==2.7.0
certifi==2019.6.16
chardet==3.0.4
decorator==4.0.10
docopt==0.6.2
docutils==0.12
html2text==2018.1.9
idna==2.7
Jinja2==2.10.1
lxml==4.3.4
libsass==0.19.2 
MarkupSafe==1.1.1
num2words==0.5.10
passlib==1.7.1
pathtools==0.1.2
phonenumbers==8.10.14
Pillow==6.1.0
psutil==5.6.3
psycopg2-binary==2.8.3
PyPDF2==1.26.0
python-dateutil==2.8.0
pytz==2016.7
PyYAML==5.1.1
reportlab==3.5.23
requests==2.20.0
six==1.12.0
urllib3==1.24.3
watchdog==0.9.0
Werkzeug==0.15.4
xlwt==1.3.0
```
**NOTE: libsass是用于解决CSS样式展示异常的问题**

执行：
```
pip3 install -r requirments.txt
```
即可！
