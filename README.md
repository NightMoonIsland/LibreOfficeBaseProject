# LibreOfficeBaseProject — LibreOffice Base 数据库版本化管理工具

## 项目简介
LibreOffice Base 生成的 `.odb` 是二进制压缩包，无法直接用 Git 做版本管理、查看修改差异。

本项目通过 **7-Zip + 批处理脚本**，将 `.odb` 文件解包为可读的文本/源码结构，实现：
- Git 正常追踪修改
- 查看版本差异
- 团队协作编辑数据库结构
- 一键重新打包为可用的 `.odb` 文件

## 文件结构
```
ProjectLBBase/
├─ repack.bat        # 打包：源码 → 生成可用的 Project.odb
├─ unpack.bat        # 解包：Project.odb → 解压为源码结构
├─ 7z.exe            # 7-Zip 命令行工具（需自行放置）
├─ ProjectLBBaseSrc/
│  └─ ProjectDB/     # 解包后的数据库源码（Git 追踪）
├─ ProjectLBBaseDst/
│  └─ Project.odb    # 最终可用的数据库文件（已 gitignore）
└─ .gitignore        # 忽略二进制 .odb 文件
```

## 使用方法

### 1. 首次/更新后 解包数据库
将 `Project.odb` 放在根目录，运行：
```
unpack.bat
```
解包内容会自动放入 `ProjectLBBaseSrc\ProjectDB`

### 2. 编辑完成后 打包生成可用数据库
运行：
```
repack.bat
```
生成的 `Project.odb` 会输出到 `ProjectLBBaseDst\`

### 3. Git 版本管理
- 只提交 **`ProjectLBBaseSrc/`** 下的源码
- `.odb` 二进制文件已自动忽略

## 依赖
- [7-Zip](https://www.7-zip.org/)
- 将 `7z.exe` 放在项目根目录

## 工作流程
1. 解包 → 修改源码/数据库结构
2. Git 提交/对比/协作
3. 打包 → 得到可运行的 .odb 文件