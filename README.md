# 新闻检索服务

基于Spring Boot实现的新闻检索服务，提供关键词搜索功能。

## 功能特性

- 支持多关键词搜索
- 支持复杂查询（过滤、排序、分页）
- 异步处理请求，支持最多200人并发
- 详细的错误处理和日志记录
- 使用Swagger提供API文档
- 支持无数据库模式启动
- 提供数据库配置Web界面
- 健康检查接口

## 技术栈

- Spring Boot 2.7.16
- Spring Data JPA
- MySQL
- Swagger 2
- Logback

## 运行环境

- JDK 1.8+
- MySQL 5.7+
- Tomcat 8.5+
- Maven 3.6+

## 快速开始

### 1. 无数据库模式

服务支持无数据库模式启动，可以在不配置数据库的情况下正常运行：
- 在没有数据库的情况下，访问主页会看到数据库状态为"未配置"
- 搜索功能会返回友好提示
- 可以通过配置页面设置数据库连接

### 2. 数据库配置

访问配置页面：
```
http://localhost:8080/news/config
```

填写数据库连接信息并保存，然后重启应用。

### 3. 编译打包

```bash
mvn clean package
```

### 4. 部署到Tomcat

将生成的`target/news-service-0.0.1-SNAPSHOT.war`文件部署到Tomcat的`webapps`目录下，重命名为`news.war`。

### 5. 访问应用

启动Tomcat后，通过以下URL访问应用：

```
http://localhost:8080/news
```

### 6. 健康检查

```
http://localhost:8080/news/health
```

## API使用示例

### 关键词搜索

**请求**：

```http
POST /api/search
Content-Type: application/json

{
  "keywords": ["关键词1", "关键词2"],
  "page": 0,
  "size": 10,
  "sortField": "created",
  "sortDirection": "desc"
}
```

**响应**：

```json
[
  {
    "name": "节目名称",
    "created": "2023-03-28T10:30:00",
    "content": "组合后的内容文本..."
  }
]
```

## 日志配置

日志文件存放在Tomcat的logs目录下，文件名为`news-service.log`。