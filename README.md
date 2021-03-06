# my-jeesite

基于jeesite进行二次开发

踩坑1:
    在页面上使用代码生成器在填写业务表配置时，如果数据库表的主键ID使用Long类型，则页面上的java类型下拉框选择Long类型，插入列不要勾选，
    这样生产的mapper.xml中的insert语句中就不会插入id列，而使用数据库的自增机制（前提时数据库id字段已经设置为自增）。
    如果数据库表主键ID使用uuid，那么页面上的java类型下拉框要选择String类型，而且插入列一定要勾选，否则执行插入数据会提示主键ID没有默认值。

踩坑2:
    删除cms模块时，访问系统首页(登陆页面)报从Session管理器中读取换成时找不到Category这个类的错误。
    原因系统使用ehcache缓存一些不常变化的数据，其中就包括cms的分类信息。解决办法是在resources/cache目录下找到ehcache-local.xml和ehcache-rmi.xml这两个文件:
    (1)删除这两个文件中和cms模块缓存有关的部分;
    (2)修改一下这两个配置文件中设置的默认缓存路径,比如把/temp/改成/temp0/:<diskStore path="../temp0/jeesite/ehcache" />
    重启下浏览器，重新访问登陆页面即可。

踩坑3:
    修改数据库sys_user表中的用户名(login_name)之后，使用修改前的用户名依然能够登陆成功。原因是用户信息在系统启动时已经缓存到本地了。
    解决办法是修改项目resources/cache目录下面的两个和ehcache有关的配置文件，将里面的如下路径随便修改成一个新的路径然后重启系统即可:
    <diskStore path="../temp/jeesite/ehcache" />

踩坑4:
    本地启动改项目后使用http://localhost:8081/druid访问是没有问题的，如果使用IP访问(比如http://172.24.148.53:8081/druid)就提示如下信息:
    Sorry, you are not permitted to view this page.
    原因是web.xml中关于druid访问路径默认配置的是127.0.0.1,修改DruidStatView中的<param-value>为本机ip地址或域名重启项目即可。

踩坑5:
    如果使用数据库的自增主键的话，在页面上使用代码生成器时，注意不要勾选插入ID列。另外由于用户模块在插入用户表时，
    会保存用户角色关系表，而保存关系表需要用户的ID，所有保存用户之后要返回数据库自增ID。





