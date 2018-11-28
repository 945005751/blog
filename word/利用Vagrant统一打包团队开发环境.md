<div align="center">
<img src="http://klkutpci.hk.richangjiaju.com/uploads/2018/11/vagrent.png">
</div>

**在Windows下开发，如果需要使用Linux系统作为开发环境，部署Linux就成为一件很让人头疼的事情。**

使用Vagrant打包环境，将Windows下Linux环境开发变的简单。

## **准备工作:**

- 下载安装 VirtualBox ：[https://www.virtualbox.org/](https://www.virtualbox.org/ "https://www.virtualbox.org/")
- 下载安装 Vagrant ：[http://www.vagrantup.com/](http://www.vagrantup.com/ "http://www.vagrantup.com/")

## **BOX准备:**
需要准备一个相关Linux box。

在 [http://www.vagrantbox.es/](http://www.vagrantbox.es/ "http://www.vagrantbox.es/") 这里下载更多不同系统甚至是已经配置好环境直接可以用的box，虽然可以直接在Vagrant直接使用网址，由Vagrant自动下载安装，但是考虑到网络情况，还是建议自行先下载好。
<div align="center">
<img src="http://klkutpci.hk.richangjiaju.com/uploads/2018/11/1250376290-58cb50fee4fba_articlex.png">
</div>

本次教程采用Ubuntu 16.04 box 作为安装演示
## **环境配置:**
安装好VirtualBox和Vagrant后，就可以进行环境配置

### **创建相关目录**
```c
mkdir -p vagrant/ubuntu1604
cd vagrant/ubuntu1604
```
`mkdir -p vagrant/ubuntu1604` 在测试过程，存在无法执行的问题

cmd命令中，发现该语句 [命令语法不正确。]

使用git命令框，命令正常执行

使用PowerShell，返回该命令P存在歧义
> mkdir : 无法处理参数，因为参数名称“p”具有二义性。可能的匹配项包括:  >-Path -PipelineVariable。
>所在位置 行:1 字符: 7
>+ mkdir -p vagrant/ubuntu
>+ CategoryInfo          : InvalidArgument: (:) [mkdir]，>ParameterBindingException
>+ FullyQualifiedErrorId : AmbiguousParameter,mkdir

**建议：可以选择手动建立文件夹，有基础的同学可以使用命令**

### **添加BOX**

`vagrant box add --name --path`
```c
vagrant box add ubuntu1604 .\ubuntu1604.box
```
name 表示指定box指定名称，比如我指定为 ubuntu ，使用base时，之后可以直接使用 vagrant init 进行初始化，如果自行指定名称，则初始化的时候需要指定box的名称。

path 是box对应的文件名，这里可以是本地保存box的路径，也可以是可以下载box的网址，如果是网址的话，Vagrant会自动启动下载。

![](http://klkutpci.hk.richangjiaju.com/uploads/2018/11/addubuntu.png)

### **Vagrant配置文件**
```c
vagrant init ubuntu1604
```
执行init命令后，会在当前文件夹创建一个Vagrant配置文件Vagrantfile。

### **开启虚拟机**
注：执行up命令前需先cd到Vagrant目录
```c
vagrant up
```
执行up命令后，终端会输出一系列启动信息，第一次启动会花费几分钟时间。
完成后可以VirtualBox查看安装好的虚拟机

### **配置Vagrant**
注：修改完后执行`vagrant reload`重启虚拟机生效配置文件。
#### **配置IP**
为了在Host机上通过浏览器访问Vagrant虚拟机，需要给虚拟机配置一个IP地址。使用文本编辑器修改Vagrant的Vagrantfile，如下：
```c
config.vm.network :private_network, ip: "192.168.33.10"
```
重启虚拟机后就可以在浏览器通过192.168.33.10就可以访问。

#### **设置共享文件夹**
因为我们在Host机上开发，那么编写代码时如果将修改同步到虚拟机实时查看效果呢？我们可以配置共享文件夹来实现，修改Vagrantfile：
```c
config.vm.synced_folder "/Users/Sam/Code/web/", "/web", create:true,
:owner => "vagrant",
:group => "www-data",
:mount_options => ["dmode=775","fmode=664"]
```
配置解释：

```c
config.vm.synced_folder host_folder vagrant_folder
```

第一个参数是Host机的文件夹路径，如果你填写的是相对路径的话，则文件夹是相对于当前虚拟机目录。
第二个参数是虚拟机的文件夹路径，这个路径必须是绝对路径。

可选参数：


```
create：Bool值。当Host机目录不存在是，是否自动创建。

group：虚拟机文件夹所属用户组

owner：虚拟机文件夹所属用户。

mount_options：挂载参数。
```


## 相关命令

|命令   |  功能 |
| ------------ | ------------ |
|vagrant init|初始化虚拟机|
|vagrant up|启动虚拟机|
|vagrant halt|关闭虚拟机|
|vagrant reload|重启虚拟机|
|vagrant ssh|登录虚拟机|
|vagrant status|查看虚拟机运行状态|
|vagrant destroy|销毁虚拟机|
|vagrant box list|查看本地Box列表|
|vagrant box add|添加Box|
|vagrant box remove|删除Box|
|vagrant package|打包虚拟机成Box|
