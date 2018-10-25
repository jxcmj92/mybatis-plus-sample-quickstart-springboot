mybatis-plus是mybatis的一款插件，它的主要作用是快速开发，省略mybatis的配置，具体的功能请参照官网。

开发环境：

springboot，maven，mybatis-plus，mysql，jdk1.8，lombok，阿里druid数据源

整合步骤：

1、在pom.xml加入相关配置

2、在resources中添加application.yml，设置mysql相关配置

3、在基类中添加表名和表名中对应的字段名

4、定义mapper接口，继承BaseMapper<T>接口，该接口中封装了Sql

5、在启动类中添加mapper扫描的包

5、调用mapper接口即可

