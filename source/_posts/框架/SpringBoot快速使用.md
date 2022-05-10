---
title: SpringBoot快速使用
date: 2022-03-25 09:11:35
tags: 框架 SpringBoot 
description: 
categories: SpringBoot
---
## 快速搭建一个SpringBoot应用
* 通过[spring initializr](https://start.spring.io/)生成
* 通过idea - new project - spring initializr 生成
* 启动生成的Application

> 此时启动报错：Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.

> 原因：@SpringBootApplication会自动注入数据源， 此时没有配置。

> 解决：@SpringBootApplication(Exclude = DataSourceAutoConfiguration.class)

```
        <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>2.6.4</version>
            <relativePath/> <!-- lookup parent from repository -->
        </parent>
```



```
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
```



```
        <build>
            <plugins>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                </plugin>
            </plugins>
        </build>
```



## 连接到mysql

添加依赖

```
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
```

application.yml中配置

```
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://IP:6306/DATABASE?characterEncoding=utf-8
    username: username
    password: password
```



## 使用mybatis-plus

1. 添加[MP](https://baomidou.com/)依赖和[Lombok](https://www.projectlombok.org/)简化代码

```
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.1</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
```

2. 编写实体Entity类和Mapper接口

```
@Data
public class Operation {
    @TableId
    private int id;
    private String operationName;

}

public interface OperationMapper extends BaseMapper<Operation> {
}

```

3. 在启动类上通过@MapperScan("package....mapper")扫描注入

4. 测试访问

   ```
   @SpringBootTest
   class ApplicationTests {
   
       @Autowired
       private OperationMapper operationMapper;
       @Test
       void contextLoads() {
           Operation operation = operationMapper.selectById(1);
           System.out.println(operation);
       }
   
   }
   ```

5. 配置xml文件

   ```
   mybatis-plus:
     mapper-locations:
       - classpath:xml/*.xml
   ```

6. 通过generator工具批量生成Entity类和Mapper接口

* 增加依赖，@RequestMapping需要spring-boot-starter-web

```
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.5.2</version>
        </dependency>
        <dependency>
            <groupId>org.apache.velocity</groupId>
            <artifactId>velocity</artifactId>
            <version>1.7</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
```



* 运行Generator

```
public class FastAutoGenerator {

    /**
     * 执行初始化数据库脚本
     */
    public static void before() throws SQLException {
        Connection conn = DATA_SOURCE_CONFIG.build().getConn();
//        InputStream inputStream = H2CodeGeneratorTest.class.getResourceAsStream("/sql/init.sql");
        ScriptRunner scriptRunner = new ScriptRunner(conn);
        scriptRunner.setAutoCommit(true);
//        scriptRunner.runScript(new InputStreamReader(inputStream));
        conn.close();
    }

    /**
     * 数据源配置
     */
    private static final DataSourceConfig.Builder DATA_SOURCE_CONFIG = new DataSourceConfig
            .Builder("jdbc:mysql://IP:3306/DATABASE?characterEncoding=utf-8", "root", "PASSWORD");

    /**
     * 执行 run
     */
    public static void main(String[] args) throws SQLException {
//        before();
        FastAutoGenerator.create(DATA_SOURCE_CONFIG)
                // 全局配置
                .globalConfig((scanner, builder) -> builder.author(scanner.apply("请输入作者名称")))
                // 包配置
                .packageConfig((scanner, builder) -> builder.parent(scanner.apply("请输入包名")))
                // 策略配置
                .strategyConfig((scanner, builder) -> builder.addInclude(scanner.apply("请输入表名，多个表名用,隔开")).entityBuilder().enableLombok())
                /*
                    模板引擎配置，默认 Velocity 可选模板引擎 Beetl 或 Freemarker
                   .templateEngine(new BeetlTemplateEngine())
                   .templateEngine(new FreemarkerTemplateEngine())
                 */
                .execute();
    }
}
```





@SpringBootApplication做了哪些事

@Controller和@RestController

参数的 多重传递方式

日期参数，统一处理





