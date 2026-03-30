# WinEventAnalyzer

基于 **.NET 8 + WPF** 的 **Windows 事件日志可视化分析工具**。支持打开 `.evtx` 导出文件，或读取本机 **Security / System / Application** 日志，对事件进行标准化、分类、聚类、会话链与风险规则检测，并在界面中给出调查结论与关键证据提示。

当前版本：**v1.0.2**（窗口标题栏会显示版本号）

---

## 功能概览

| 能力           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| **离线分析**   | 选择 `.evtx` 文件进行解析与展示                              |
| **本机日志**   | 读取 Security、System、Application（各通道最近若干条，可配置） |
| **事件标准化** | 统一时间、EventID、通道、用户、源 IP、关联键等字段           |
| **语义分类**   | 认证、账号管理、进程、服务、计划任务、策略审计等（含未知 ID 的启发式分类） |
| **聚类合并**   | 按用户 / IP / LogonId 等 + 时间窗口聚合                      |
| **会话链**     | 以 LogonId 为主线的会话视图，附阶段摘要与简要结论            |
| **风险规则**   | 如：疑似暴力破解、非典型时段登录、账号创建+加组链等（可随版本扩展） |
| **调查结论**   | 总体风险评分、结论文案、关键证据条目                         |
| **筛选**       | 按用户、源 IP、类别、日期范围过滤各视图                      |

---

## 运行环境

- **操作系统**：Windows 10 / 11 或 Windows Server（x64）
- **运行时**：
  - 开发调试：安装 [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
  - 发布单文件：生成的 exe 为 **自包含（self-contained）**，目标机**可不单独安装** .NET 运行时（体积较大）

---

## 快速开始

### 从源码运行

```powershell
cd WinEventAnalyzer
dotnet run
```

### 编译 Debug

```powershell
dotnet build
```

### 发布单文件 exe（自包含、win-x64）

```powershell
dotnet publish -c Release
```

输出目录：`bin/Release/net8.0-windows/win-x64/publish/WinEventAnalyzer.exe`

---

## 使用说明

1. **分析导出文件**：点击「选择 .evtx 文件并分析」，选择从事件查看器或其它工具导出的 `.evtx`。
2. **分析本机日志**：点击「分析本机实时日志」。
3. **权限建议**：
   - 读取 **Security** 日志通常需要 **以管理员身份运行** 本程序。
   - 确保系统服务 **Windows 事件日志（Windows Event Log）** 已启动。
4. 若本机某通道无法打开，程序会尽量跳过该通道并继续分析其它通道；界面或弹窗中会提示警告信息。

---

## 目录结构（简要）

```
WinEventAnalyzer/
├── App.xaml / App.xaml.cs
├── MainWindow.xaml / MainWindow.xaml.cs    # 主界面与筛选
├── Models/                                   # 数据模型（事件、聚类、会话、风险、结论等）
├── Services/
│   └── EventLogAnalyzer.cs                 # 日志读取、标准化、聚类、规则与评分
├── scripts/
│   └── make-icon.ps1                       # 可选：生成 app.ico
├── app.ico
├── WinEventAnalyzer.csproj
└── README.md
```

---

## 技术栈

- **语言 / 框架**：C#，**.NET 8**，**WPF**
- **日志 API**：`System.Diagnostics.Eventing.Reader`（`EventLogReader` / `EventLogQuery`）；在部分环境下对 **经典 `System.Diagnostics.EventLog`** 做备用读取以提高兼容性

---

## 已知限制

- 本工具面向**辅助排查与初筛**，不能替代企业级 SIEM / 专业取证工具。
- 风险规则与评分为「启发式」，实际环境需结合**基线、策略与人工复核**。
- 本机「最近 N 条」为性能与体验折中，**并非全量历史**；全量分析请优先使用 `.evtx` 导出文件。

---

## 许可证

本项目采用 **MIT License**，版权所有 (c) 2026 **Miles**，全文见 [`LICENSE`](LICENSE)。

### 若使用 GitHub 网页添加许可证（可选）

1. 在 GitHub 打开仓库 → **Add file** → **Create new file**  
2. 文件名填 `LICENSE`，页面会出现 **Choose a license template**  
3. 选择 **MIT License** 等模板，填写年份与版权人，提交即可。

---

## 反馈与贡献

欢迎通过 Issue / Pull Request 提交问题与改进建议。若报告「本机日志读取失败」，请尽量说明 **Windows 版本**、是否**管理员运行**、以及窗口标题栏显示的**程序版本号**。
