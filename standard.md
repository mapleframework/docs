# 规范

## 命名规范

* Branch（后端解决方案）
  * 模块解决方案和命名空间的命名构成：Maple.Branch.{module}Module
  * 模块的层：`Shared` `Common` `Domain` `EfCore` `MongoDb` `Applicatoin` `AppService` `WebApi` `WebClient` `WebHost` ...
  * Module类文件：{module}{层名}Module
* Leaf（前端解决方案）
  * 模块的命名：@maple.leaf/ng.{module}

## 模块目录

* `host`: 模块`web`部分（简单查看，测试用）
* `src`: 管理平台与用户平台通用部分（层：`Shared` `Common` `Domain` `EfCore` `MongoDb`）
* `test`: 单元测试
* `Solution Items`: 项目（解决方案）其它文件

## editorconfig

* 每个项目都需要有`editorconfig`文件（参考：[dotnet/runtime](https://github.com/dotnet/runtime/blob/master/.editorconfig)）

## 时区

* ~~前后端统一使用 `DatetimeOffset`，前端显示再转本地时间（待实现）~~
