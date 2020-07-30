# 脚本

> ps: 实际中，请自行将注释删除

## 快速发布nuget

> 前提条件：github actions workflows, shell

> 可参考：[mapleframework/auditlog](https://github.com/mapleframework/auditlog)

### shell

* pack 打包

```shell
# 查找 ../branch/src/ 目录下所有后缀 `*.csproj` 的文件，并循环
for csproj in $(find '../branch/src/' -type f -name *.csproj)
do
    # 打包，并将打包后的文件放入当前目录的nupkgs文件夹下
    dotnet pack $csproj -c Release --no-restore -o nupkgs
done
```

* publish 发布

```shell
# 设置一个变量，保存存放待发布包的文件夹的名字
nupkgs='./nupkgs'

# 拿到执行shell脚本的第二个参数（如：sh publish.sh {key}, $1 就是{key}）
key=$1

# 循环文件夹下所有文件名
for pack in $(ls $nupkgs)
do
    # 发布
    dotnet nuget push ./nupkgs/$pack -k $key -s https://api.nuget.org/v3/index.json --skip-duplicate
done
```

### github actions workflows

```yml
name: publish
on:
  # 一个新的rel发布后执行
  release:
    types: [published]

jobs:
  publish_nuget:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@main
      - uses: actions/setup-dotnet@main
        with:
          dotnet-version: 3.1.x

      - name: build
        working-directory: ./shells
        run: sh build.sh

      - name: pack
        working-directory: ./shells
        run: sh pack.sh

      - name: publish
        working-directory: ./shells
        # 需要一个 github secret 参数作为发布 key
        env:
          Key: ${{ secrets.PUBLISH_NUGET_KEY }}
        # 执行 publish.sh 并传入变量
        run: sh publish.sh "$Key"
```

## liunx

* 删除目录下所有文件: `rm -rf {path}` （快速删除obj和bin下的文件）
