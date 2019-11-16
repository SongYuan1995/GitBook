# GitBook使用教程

## 安装

### 1.安装 Node.js 

[地址]: https://nodejs.org/en/

### 2. 安装 GitBook 

```shell
npm install -g gitbook-cli
```

### 3.检查安装情况

```
gitbook -V
```

## 

## 使用

### 1.初始化

```
gitbook init
```

### 2.启动服务

```shell
gitbook serve
```

出现错误：

Error: ENOENT: no such file or directory, stat 'E:\gitBooks\testBook\_book\gitbook\gitbook-plugin-fontsettings\fontsettings.js

解决方式：

用户目录下找到以下文件。

.gitbook\versions\3.2.3\lib\output\website\copyPluginAssets.js

```shell
# replace all
confirm: true
# with
confirm: false
```



