# Airflow.Markdown

面向常见 .NET 桌面框架的高性能、支持流式渲染的 Markdown 渲染器。

本仓库是四合一（monorepo）结构，一次构建/发布四个 NuGet 包：

| 包 | 目标框架 | 说明 |
| --- | --- | --- |
| `Airflow.Markdown.Abstractions` | `net10.0` | 核心抽象接口与共享类型 |
| `Airflow.Markdown.Avalonia` | `net10.0` | Avalonia 控件 |
| `Airflow.Markdown.Wpf` | `net10.0-windows` | WPF 控件 |
| `Airflow.Markdown.Winui3` | `net10.0-windows10.0.19041.0` | WinUI 3 控件 |

## 目录结构

```
Airflow.Markdown.slnx        # 统一解决方案
Directory.Build.props        # 共享编译/打包属性（版本、作者、License、SourceLink）
Directory.Packages.props     # 中央包版本管理（CPM）
src/
  Airflow.Markdown.Abstractions/
  Airflow.Markdown.Avalonia/
  Airflow.Markdown.Wpf/
  Airflow.Markdown.Winui3/
```

## 版本管理与分支模型

分支与版本号一一对应，与 `Airflow.MSDI.AutoRegister` 使用同一套流程。

| 分支 | 用途 | 版本号 |
| --- | --- | --- |
| `main` | 默认分支、发布镜像；只接受 `release-*` 合入 | 与最新正式版一致 |
| `dev-1.0` | 1.0.x 开发线，feature 从这里切出 | `1.0.0-dev` 起 |
| `dev-1.1` | 1.1.x 开发线，1.1 开工时从 `dev-1.0` 切出 | `1.1.0-dev` 起 |
| `release-1.x` | 1.x 维护线；收 bugfix、打正式版 tag | `1.0.0`、`1.0.1`… |
| `release-2.x` | 2.x 维护线 | `2.0.0`、`2.0.1`… |

发布流程：

```
feature/* ──PR──▶ dev-1.0 ──合并──▶ release-1.x ──tag v1.0.0──▶ main
                    │                                              │
                    └─(1.1 开工)──▶ dev-1.1 ──▶ release-1.x        └─▶ 触发 NuGet 发布
hotfix/* ──从 release-1.x 切出──▶ release-1.x ──▶ main
```

版本号规则：

- 唯一的版本源是 [`Directory.Build.props`](Directory.Build.props) 里的 `<Version>`，四个包共用同一版本、一起发布。
- 开发分支使用预发布号：`1.0.0-dev`、`1.0.0-alpha.1` 等，不发布到正式渠道。
- `release-*` 分支发正式版，每次发版 bump patch。
- 发版步骤：bump `<Version>` → 合并到 `release-x.x` → 打 `vX.Y.Z` tag → 合并 `release-x.x` 到 `main` → CI 自动 `pack` 并推送四个包。
- 修复已发布版本：从 `release-x.x` 切 `hotfix/*`，修完合回 `release-x.x` 再走发版流程。

分支命名约定：`dev-<major>.<minor>`、`release-<major>.x`、`feature/<简短描述>`、`hotfix/<简短描述>`。

当前 1.0.0 的实施任务清单见 [`.task/1.0.0.impl.md`](.task/1.0.0.impl.md)。

## 构建与发布

```bash
# Linux 上可构建的包（Abstractions、Avalonia）
dotnet build src/Airflow.Markdown.Abstractions/Airflow.Markdown.Abstractions.csproj
dotnet build src/Airflow.Markdown.Avalonia/Airflow.Markdown.Avalonia.csproj

# Windows 上构建 WPF / WinUI3
dotnet build Airflow.Markdown.slnx -c Release

# 打包四个 NuGet 包
dotnet pack Airflow.Markdown.slnx -c Release -o artifacts

# 发布四个包
dotnet nuget push "artifacts/*.nupkg" --api-key <NUGET_API_KEY> --source https://api.nuget.org/v3/index.json
```

## License

AGPL-3.0-or-later，详见 [LICENSE](LICENSE)。
