## Ubuntu下git服务器的搭建

根据我的建议，实验室新增了一台服务器，在上面搭建git服务器，从而可以满足大家的需求。

### 一、安装`ssh`

```shell
sudo apt-get install openssh-server openssh-client
```

### 二、安装`git`

```shell
sudo apt-get install git
```

### 三、新增用户`git`

```shell
sudo adduser git
```

同时，为了安全起见，将`用户git`的`默认shell`改为`git-shell`

```shell
sudo vim /etc/passwd
```

将`用户git`的shell改为`git-shell`

```sh
git:x:1000:1000:git,,,:/home/git:/usr/bin/git-shell
```

### 四、切换到`用户git`

```shell
sudo - git
```

### 五、新建目录`.shh`

切换到git用户目录下，新建文件夹`.ssh`

```shell
mkdir .ssh
```

### 六、在本地生成公私钥，并将公钥添加到`authorized_keys`

不存在该文件的话，新建

```shell
touch authorized_keys
```

然后，将公钥添加到其中

``` shell
echo 公钥 >> authorized_keys #或者使用其他方式添加，保证一行一个密钥
```
### 七、创建空的git库

```shell
git init --bare 仓库名称.git
```

### 八、git clone

```
git clone git@服务器IP:仓库名称.git
```







