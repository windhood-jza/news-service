# 新闻检索服务


### 接口服务需求记录

**1. 数据源**
   - 数据库类型：MySQL
   - 数据类型：多个表需要关联搜索，存储了节目的语音转换文字的文字信息，节目名称，入库时间等。

**2. 接口功能**
   - 查询功能：通过输入的1个或者多个关键词，查找包含这些关键词的语音文字信息，并关联找出对应的节目名称，入库时间等。
   - 支持复杂查询：需要支持过滤、排序、分页。

**3. 接口安全性**
   - 不需要身份验证和授权机制。

**4. 接口性能**
   - 响应时间：尽可能留有富足的响应时间，超时时间尽可能长。
   - 并发请求：有处理需求。

**5. 接口格式**
   - 返回数据格式：JSON
   - 支持多种数据格式：包括字符串，大文本，时间类型等。

**6. 错误处理**
   - 返回详细的出错信息，并记录在日志中。

**7. 接口文档**
   - 需要生成接口文档，具体工具和格式由开发者选择。

**8. 部署和环境**
   - 部署环境：本地服务器，使用Tomcat。
   - 技术栈：Spring Boot

**9. 数据过滤**
   - 需要在从数据库取回信息后进行数据过滤，去除不需要的内容。

**10. 数据库结构**
   - 表名：cob_program，字段名：OBJECTID, FIELD1079
   - 表名：com_basicinfo，字段名：ID, NAME, CREATED
   - 关联关系：OBJECTID=ID

**11. 查询示例**
   - 通过关键词在cob_program表的FIELD1079字段内容里查找，找出所有的记录。
   - 用对应的OBJECTID记录去com_basicinfo表里反查对应记录的NAME, CREATED。
   - 结果格式：JSON，包含name, created和组合后的content内容。

**12. 并发处理需求**
   - 最大支持200人的同时查询，响应最好是异步的。

**13. 日志记录要求**
   - 日志放在Tomcat的默认目录下。

**14. 接口文档**
   - 使用Swagger生成接口文档。

# 数据库配置
spring.datasource.url=jdbc:mysql://localhost:3306/your_database_name
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# JPA配置
spring.jpa.hibernate.ddl-auto=none
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect

# 服务器配置
server.port=8080
server.tomcat.max-threads=200
server.connection-timeout=300000

# 日志配置
logging.file.path=${catalina.base}/logs
logging.file.name=${logging.file.path}/news-service.log
logging.level.com.news.service=INFO

# Swagger配置
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
            .select()
            .apis(RequestHandlerSelectors.basePackage("com.news.service.controller"))
            .paths(PathSelectors.any())
            .build()
            .apiInfo(apiInfo());
    }
    
    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
            .title("新闻检索服务 API")
            .description("提供基于关键词的新闻内容检索服务")
            .version("1.0.0")
            .build();
    }
} 

**15. 无数据库启动模式**
   - 应用必须能够在没有数据库连接的情况下正常启动和运行
   - 提供明显的日志信息指示应用处于"无数据库模式"
   - 在UI界面上显示应用当前的数据库连接状态
   - 搜索功能在无数据库模式下应提供友好的提示信息而非错误页面

**16. 数据库配置界面**
   - 提供Web界面用于配置数据库连接信息
   - 配置信息应包括：数据库URL、用户名、密码等必要参数
   - 提供数据库连接测试功能，验证配置的有效性
   - 配置信息应保存到外部配置文件，支持应用重启后读取

**17. 配置状态展示**
   - 主页顶部应显示当前数据库连接状态
   - 状态应包括：已连接、未配置、配置错误等多种状态
   - 根据不同状态显示不同的操作提示和界面元素

**18. 健康检查接口**
   - 提供/health接口返回应用当前状态，包括数据库连接状态
   - 数据库状态应返回ENABLED或DISABLED

**19. 重启机制**
   - 配置数据库后需要重启应用以激活数据库功能
   - 重启后应自动读取外部配置文件并尝试连接数据库
   - 提供明确的重启指南和用户提示

**20. 全文搜索优化**
   - 使用MySQL的全文搜索功能提高搜索效率
   - 简化查询接口设计，提高代码可维护性