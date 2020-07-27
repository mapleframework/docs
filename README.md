# Maple Framework Docs

## 命名规范

* Branch（后端）
  * 模块解决方案和命名空间的命名构成：Maple.Branch.{module}Module
  * 模块的层：`Shared` `Common` `Domain` `EfCore` `MongoDb` `Applicatoin` `AppService` `WebApi` `WebClient` `WebHost`
  * Module类文件：MapleBranch{module}{层名}Module
* Leaf（前端）
  * 模块的命名：@maple.leaf/ng.{module}

## editorconfig

* 每个项目都需要有`editorconfig`文件
