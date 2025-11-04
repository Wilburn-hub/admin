# Repository Guidelines

## Project Structure & Module Organization
- 根目录包含 Maven 构建文件 `pom.xml`、Docker 资产以及入口类 `src/main/java/com/cool/CoolApplication.java`。
- 业务模块位于 `src/main/java/com/cool/modules`（示例：user、task、plugin），通用组件集中在 `src/main/java/com/cool/core`（cache、security、tenant、code-gen 等）。
- 配置与模板位于 `src/main/resources`，具体环境配置分别在 `application-local.yml` 与 `application-prod.yml`，初始化数据在 `resources/cool/data`。
- 测试代码与主目录结构一致，存放于 `src/test/java/com/cool`。

## Build, Test, and Development Commands
- `mvn compile`：触发 MyBatis-Flex APT 生成，首次启动之前执行。
- `mvn spring-boot:run -Dspring-boot.run.profiles=local`：使用 local 配置启动后端，并自动建表。
- `mvn clean package -DskipTests`：打包生成可运行的 Spring Boot Jar。
- `mvn test`：运行 JUnit/Spring Boot 测试套件。
- `docker-compose up -d mysql redis`：启动可选的本地 MySQL 与 Redis 服务。

## Coding Style & Naming Conventions
- 统一使用 Java 17、Lombok 简化样板代码，缩进为四个空格，花括号与声明同列。
- 包名保持小写（`com.cool.core.cache`），类名使用 PascalCase，bean 与属性使用 camelCase。
- 控制器、服务、数据访问类分别置于 `modules/*/controller|service|mapper`，SQL 统一写在 Flex Mapper 中。
- 默认使用 `@Slf4j` 输出日志，避免 System.out。

## Testing Guidelines
- 测试类位于 `src/test/java/com/cool`，使用 JUnit 5 与 Spring Boot 测试切片。
- 命名规范：类名以 `*Tests` 结尾，方法遵循 `methodUnderTest_condition_expectedResult`。
- 推送前运行 `mvn test`，覆盖新增服务逻辑与定制 Flex 查询的正反用例；涉及数据访问时启用 `local` profile 复用建表流程。

## Commit & Pull Request Guidelines
- 遵循 Conventional Commits（如 `feat:`, `fix:`, `chore:`），scope 以模块命名：`feat(user): add login audit`。
- 提交信息使用祈使语态，必要时在正文补充动机或迁移说明。
- PR 描述需包含变更摘要、关联 issue、测试结果（`mvn test` 输出）以及 UI 受影响时的截图；指派熟悉对应模块的审阅者，并标注数据库或配置变更。

## Configuration & Security Tips
- 禁止提交敏感凭据；通过环境变量或忽略的 `.env.local` 覆盖数据源配置，并分享前清理 `application-local.yml`。
- Redis 持久化仅在测试相关功能时启用，`docker-compose` 暴露端口需限制在可信网络内。

## 中文贡献规则速览
- 代码目录：业务模块放在 `modules`，基础能力放在 `core`，配置与资源位于 `resources`。
- 开发流程：先 `mvn compile`，本地运行用 `spring-boot:run -Dspring-boot.run.profiles=local`。
- 编码规范：Java 17 + Lombok，四空格缩进，类名大驼峰，方法保持精简。
- 测试要求：新增逻辑需补 `*Tests`，推送前执行 `mvn test` 并关注数据库场景。
- 提交流程：采用 Conventional Commits，PR 需附测试结果与必要截图，说明任何环境更改。
