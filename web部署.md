# 云服务器部署（阿里云）

## 1.安装环境

使用sudo apt安装python、mysql-server等必要环境

## 2.导出sql脚本，在云服务器上使用sql脚本创建数据库

- 在本机上执行```mysqldump -u root -p covid_info > covid_info.sql```导出covid_info数据库的sql脚本
注意：在powershell中导出会使导出的sql文件中产生一些特殊符号，导致导入时出错，要在cmd中进行导出

- 使用ftp把sql文件复制到云服务器中，在云服务器中创建一个新的数据库，并use进入该数据库，执行```source 'sql文件路径'```，导入数据库

## 3.部署Django

- 把Django项目文件复制到云服务器中，并下载需要的包

- 在setting.py中修改allowed_host=['*']

- 执行```python manage.py runserver 0.0.0.0:8080```运行服务器

- 在阿里云实例中修改安全组配置，将8080端口加入，才能够从浏览器中在8080端口对服务器进行访问

- 可能出现的问题**django.db.utils.InternalError: (1698, "Access denied for user 'root'@'localhost'")**
  解决方法：在mysql中新建一个用户，并修改django项目中的user为新创建的用户

```sql
  create user '用户名'@'%' identified by '密码';  # 创建用户
  grant all on *.* to '用户名'@'%';  # 授权
  flush privileges;  # 刷新权限
```

## 4.配置nginx

- 在vue项目里运行```npm run build```将项目打包

- 在云服务器中下载nginx，并进入/etc/nginx,打开nginx.conf文件进行配置

- 在http中设置添加server设置节点

```conf
http{
  server{
    location \ {
      root 'html文件所在目录';
      index index.html;
    }   
  }
}
```
