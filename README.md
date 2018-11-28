# vagrant-php-devenv
基于vagrant搭建php开发环境

## 使用vagrant+virtualBox搭建开发环境的优点
1. 更灵活的扩展安装。
2. 更方便的软件安装与环境搭建。毕竟是系统级的，有更高的权限。
3. 与宿主机环境隔离，即便环境折腾废了，destroy后，重新init就可以
4. 共享项目目录，在宿主机开发的代码，会直接同步在虚拟机中，无需部署代码
5. 更灵活的网络配置，可配置使用宿主机的代理，轻松实现外网资源的下载，比如加速github项目下载速度或composer国外镜像站下载速度。

## package下载
> [百度网盘](https://pan.baidu.com/s/1KX3Ho5PwCbxlBCRwIa0Xig)
> 密码：jmiu

## 安装vagrant和virtualBox
> [vagrant下载地址](https://www.vagrantup.com/downloads.html)

> [VirtualBox下载地址](https://www.virtualbox.org/wiki/Downloads)

## 使用方式
1. 进入本地项目目录(需要共享进虚拟机的目录)
2. 执行以下命令
```shell
#安装vagrant插件
vagrant plugin install vagrant-vbguest
# 导入package.box镜像文件
vagrant box add php-dev-env centos7.3-php7.2.12-nginx1.12.2.box
# 初始化，生成Vagrantfile配置文件
vagrant init php-dev-env
```

3. 编辑目录内的Vagrantfile文件
```shell
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "php-dev-env"
  config.vm.network "private_network", type: "dhcp"
  config.vbguest.auto_update = false
  config.vbguest.no_remote = true
  config.vm.synced_folder ".", "/www",owner: "vagrant",group: "vagrant",mount_options:["dmode=777","fmode=777"]
end
```

4. 启动虚拟机
```shell
vagrant up
#自动挂载插件
vagrant vbguest --auto-reboot
```

5. 进行虚拟机
``` shell
vagrant ssh
```

## 系统环境
用户名密码
```shell
username  password
vagrant   vagrant
root      vagrant
```

## 项目挂载路径
项目目录 `/www/`
nginx虚拟机配置目录 `/www/nginx.conf.d/`

## PHP环境
版本
```shell
$ php -v
PHP 7.2.12 (cli) (built: Nov 28 2018 02:21:36) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
```

常用路径
```shell
php路径： /usr/local/php7/
phpize路径： /usr/local/php7/bin/phpize
php-config路径： /usr/local/php7/bin/php-config
php.ini路径： /usr/local/php7/lib/php.ini

/usr/local/php7/etc/php-fpm.conf
/usr/local/php7/etc/php-fpm.d/www.conf
/etc/init.d/php-fpm
```

php服务管理
```shell
service php-fpm restart
service php-fpm start
service php-fpm stop
service php-fpm status
```

扩展安装
> [CenOS7环境安装PHP7扩展](https://hanxv.cn/archives/25.html)

## nginx版本
```shell
$ nginx -v
nginx version: nginx/1.12.2
```

nginx服务管理
```shell
service nginx restart
service nginx start
service nginx stop
service nginx status
```
