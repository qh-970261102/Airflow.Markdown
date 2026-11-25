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
