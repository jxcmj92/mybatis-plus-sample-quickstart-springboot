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

项目架构：



具体步骤代码：

1、在pom.xml加入相关配置

<!-- spring-boot-starter-parent 和 mybatis-plus-boot-starter 有版本要求-->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.3.RELEASE</version>
</parent>


<dependencies>
    <!-- spring-boot -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>


    <!--mybatis-plus自动的维护了mybatis以及mybatis-spring的依赖，
      在springboot中这三者不能同时的出现，避免版本的冲突，表示：跳进过这个坑-->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.0.1</version>
    </dependency>

    <!-- 引入Druid依赖，阿里巴巴所提供的数据源 -->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.0.29</version>
    </dependency>

    <!-- 提供mysql驱动 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.38</version>
    </dependency>

    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.16.16</version>
    </dependency>

</dependencies>
2、在resources中添加application.yml，设置mysql相关配置

server:
  port: 2525

# 该配置的名称是固定的，不可以改变，否则将不能自动加载到数据源中
spring:
  datasource:
    # 使用druid数据源
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://192.168.3.172:3306/poc?allowMultiQueries=true&useUnicode=true&characterEncoding=UTF-8
    username: devuser
    password: dev123
    filters: stat
    maxActive: 20
    initialSize: 1
    maxWait: 60000
    minIdle: 1
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: select 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true
    maxOpenPreparedStatements: 20
3、在基类中添加表名和表名中对应的字段名

@Data
@NoArgsConstructor
@AllArgsConstructor
@TableName("tb_test_student") //对应表名
public class Student implements Serializable {

    //对应id，可不填
    @TableId(value = "id")
    private int id;

    //对应字段名，如果属性名和字段名一致，可不填
    @TableField("name")
    private String name;

    private String school;

    private String city;

    //表示表中没有这个字段，如果不加该注释，会抛异常
    @TableField(exist = false)
    private String address;
}
4、定义mapper接口，继承BaseMapper<T>接口，该接口中封装了Sql

public interface StudentMapper extends BaseMapper<Student> {

}
5、在启动类中添加mapper扫描的包

@SpringBootApplication
@MapperScan("com.springboot.mybatisplus.mapper")
public class App {

    public static void main(String[] args) {
        SpringApplication.run(App.class,args);
    }
}
5、调用mapper接口即可

@RestController
@RequestMapping("/mybatisplus")
public class TestMain {
    
    @Autowired
    private StudentMapper studentMapper;
    
    @GetMapping("/list")
    public List<Student> list(){
        List<Student> students = studentMapper.selectList(null);
        return students;
    }


    @GetMapping("/save")
    public String save(){
        Student student = new Student();
        student.setId(2);
        student.setCity("杭州");
        student.setName("马云");
        student.setSchool("杭州师范");
        studentMapper.insert(student);
        return "success";
    }

}
具体的API请参照mybatis-plus官网：http://mp.baomidou.com/guide/

github地址：
