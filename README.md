# HiedaCamellia.ProjectBaseGame

[![Chickensoft 徽章][chickensoft-badge]][chickensoft-website] [![Discord][discord-badge]][discord] [![阅读文档][read-the-docs-badge]][docs] ![行覆盖率][line-coverage] ![分支覆盖率][branch-coverage]

这是一个用于 Godot 4 的 C# 游戏模板，基于 [Chickensoft.GodotGame]("https://github.com/chickensoft-games/GodotGame")，
并使用 [HiedaCamellia.ProjectBase]("https://github.com/HiedaCamellia/ProjectBaseGame") 添加了一些额外的功能。
包含调试启动配置、测试（本地和 CI/CD）、代码覆盖率、依赖更新检查和拼写检查，开箱即用！

---

<p align="center">
<img alt="带有 Chickensoft Logo 的纸箱" src="icon.png" width="200">
</p>

## 🥚 入门指南

此模板允许您轻松创建 Godot 4 的 C# 游戏。Microsoft 的 `dotnet` 工具使您可以轻松创建、安装和使用模板。

```sh
# 安装此模板
dotnet new install Chickensoft.GodotGame

# 基于此模板生成新项目
dotnet new chickengame --name "MyGameName" --param:author "我的名字"

cd MyGameName
dotnet build
```

## 💁 获取帮助

> 待完善

## 🏝 环境设置

为了使提供的调试配置和测试覆盖率正常工作，您必须正确设置开发环境。[Chickensoft 设置文档][setup-docs] 描述了如何使用 Chickensoft 的最佳实践建议来设置您的 Godot 和 C# 开发环境。

### VSCode 设置

此模板包含一些 Visual Studio Code 设置，位于 `.vscode/settings.json` 中。这些设置有助于在 Windows（Git Bash、PowerShell、命令提示符）和 macOS（zsh）上的终端环境，以及修复 Omnisharp 的一些语法着色问题。您还会发现一些设置，这些设置启用了 Omnisharp 中的编辑器配置支持和 .NET Roslyn 分析器，以获得更愉快的编码体验。

> 请仔细检查提供的 VSCode 设置是否与您现有的设置冲突。

## .NET 版本控制

包含的 [`global.json`](./global.json) 指定了游戏应使用的 .NET SDK 和 `Godot.NET.Sdk` 的版本。使用 `global.json` 文件允许 [Renovatebot] 在 [GodotSharp] 发布新版本时，为您的存储库提供自动依赖更新拉取请求。

## 👷 测试

包含了一个示例测试 `test/src/GameTest.cs`，展示了如何使用 [GoDotTest] 和 [godot-test-driver] 为您的包编写测试。

> [GoDotTest] 是一个易于使用的 Godot 和 C# 测试框架，允许您从命令行运行测试、收集代码覆盖率并在 VSCode 中调试测试。

测试直接在游戏中运行。`.csproj` 文件已经预先配置，以防止测试脚本和仅用于测试的包依赖项包含在游戏的发布版本中！

在 CI/CD 上，[mesa] 的软件图形驱动程序模拟了一个虚拟图形设备，供 Godot 渲染，允许您在无头环境中运行视觉测试。

## 🏁 应用程序入口点

`Main.tscn` 和 `Main.cs` 场景和脚本文件是游戏的入口点。通常，除非您正在进行高度自定义的操作，否则您可能不需要修改这些文件。

如果游戏正在运行发布版本，`Main.cs` 文件将立即将场景更改为 `src/Game.tscn`。如果游戏正在调试模式下运行 *并且* GoDotTest 已收到正确的命令行参数以开始测试，游戏将切换到测试场景并将控制权交给 GoDotTest 以运行游戏的测试。

通常，优先编辑 `src/Game.tscn` 而不是 `src/Main.tscn`。

提供的调试配置在 `.vscode/launch.json` 中，允许您轻松调试测试（或仅调试当前打开的测试，前提是其文件名与其类名匹配）。

## 🚦 测试覆盖率

代码覆盖率需要首先安装一些 `dotnet` 全局工具。您应该从项目目录的根目录安装这些工具。

项目根目录中的 `nuget.config` 文件允许从 coverlet 夜间分发中安装正确版本的 `coverlet`。在 [coverlet 发布一个稳定版本并修复了与 Godot 4 的兼容性问题之前][coverlet-issues]，覆盖 coverlet 版本将是必要的。

```sh
dotnet tool install --global coverlet.console
dotnet tool update --global coverlet.console
dotnet tool install --global dotnet-reportgenerator-globaltool
dotnet tool update --global dotnet-reportgenerator-globaltool
```

> 在 Apple Silicon 计算机上，通常需要运行 `dotnet tool update` 以确保工具正确安装。

您可以通过运行 bash 脚本 `coverage.sh` 来收集代码覆盖率并生成覆盖率徽章（在 Windows 上，您可以使用随 git 一起提供的 Git Bash shell）。

```sh
# 第一次使用时必须授予覆盖率脚本运行权限。
chmod +x ./coverage.sh

# 运行代码覆盖率：
./coverage.sh
```

您还可以通过 VSCode 运行测试覆盖率，方法是打开命令面板并选择 `Tasks: Run Task`，然后选择 `coverage`。

如果您在 Windows 上遇到 `coverlet` 找不到 .NET 运行时的问题，可以使用 PowerShell 脚本 `coverage.ps1`。

```ps
.\coverage.ps1
```

## ⏯ 运行项目

包含多个 Visual Studio Code 的启动配置文件：

- 🕹 **调试游戏**

  以调试模式运行游戏，允许您设置断点并检查变量。

- � **调试当前场景**

  调试游戏并加载与 VSCode 中当前选中的 C# 文件**同名**且**位于同一路径**的场景：例如，名为 `MyScene.tscn` 的场景必须与 `MyScene.cs` 位于同一目录中，并且您必须在运行启动配置文件之前将 `MyScene.cs` 选为 VSCode 中的活动选项卡。

  如果 GoDotTest 能够找到同名且位于同一位置的 `.tscn` 文件，它将以调试模式运行游戏并加载该场景。

  > 自然，Chickensoft 建议将场景命名为它们使用的 C# 脚本，并将它们保存在同一目录中，以便您可以利用此启动配置文件。
  >
  > ⚠️ 很容易重命名脚本类但忘记重命名场景文件，反之亦然。当这种情况发生时，此启动配置文件将根据脚本的名称传入场景文件的*预期*名称，但 Godot 将无法找到具有该名称的场景，因为脚本名称和场景名称不相同。

- 🧪 **调试测试**

  以调试模式运行游戏，指定 GoDotTest 运行测试所需的命令行标志。调试与往常一样工作，允许您在游戏的 C# 测试文件中设置断点。

- 🔬 **调试当前测试**

  调试游戏并加载与 VSCode 中当前选中的 C# 文件**同名**的测试类：例如，名为 `MyTest.cs` 的测试文件必须包含名为 `MyTest` 的测试类，并且您必须在运行启动配置文件之前将 `MyTest.cs` 选为 VSCode 中的活动选项卡。

  > ⚠️ 很容易重命名测试类但忘记重命名测试文件，反之亦然。当这种情况发生时，此启动配置文件将传入文件名，但 GoDotTest 将无法找到具有该名称的类，因为文件名和类名不相同。

请注意，每个启动配置文件都会在调试游戏之前触发构建（请参阅 `./.vscode/tasks.json`）。

> ⚠️ **重要提示：** 您必须为上述启动配置设置 `GODOT` 环境变量。如果尚未设置，请参阅 [Chickensoft 设置文档][setup-docs]。

## 🏭 CI/CD

此游戏包含各种 GitHub Actions 工作流以帮助开发。

### 🚥 测试

测试直接在 GitHub 运行器机器上运行（使用 [chickensoft-games/setup-godot]），每次推送到存储库时都会运行。如果测试未通过，工作流也将无法通过。

您可以在 [`.github/workflows/visual_tests.yaml`](.github/workflows/visual_tests.yaml) 中配置要在哪些模拟图形环境（`vulkan` 和/或 `opengl3`）上运行测试。

目前，测试只能从 `ubuntu` 运行器运行。如果您知道如何在工作流中安装 mesa 和虚拟窗口管理器在 macOS 和 Windows 上运行，我们很乐意听取您的意见！

测试通过从命令行运行 `Chickensoft.GodotGame` 中的 Godot 测试项目并传递相关参数给 Godot，以便 [GoDotTest] 可以发现并运行测试。

### �‍🏫 拼写检查

每次推送到存储库时都会运行拼写检查。拼写检查工作流设置可以在 [`.github/workflows/spellcheck.yaml`](.github/workflows/spellcheck.yaml) 中配置。

建议使用 VSCode 的 [Code Spell Checker][cspell] 插件来帮助您在提交之前捕获拼写错误。如果需要将单词添加到字典或忽略某个路径，您可以编辑项目的 `cspell.json` 文件。

您还可以通过 VSCode 将单词添加到本地 `cspell.json` 文件中，方法是悬停在拼写错误的单词上并选择 `Quick Fix...`，然后选择 `Add "{word}" to config: cspell.json`。

![修复拼写](docs/spelling_fix.png)

### 🗂 版本更改

包含的工作流 [`.github/workflows/version_change.yaml`](.github/workflows/version_change.yaml) 可以手动调度，以打开一个拉取请求，将 `Chickensoft.GodotGame.csproj` 中的版本号替换为您在工作流输入中指定的版本。

![版本更改工作流](docs/version_change.png)

### 📦 发布到 Nuget

包含的工作流 [`.github/workflows/publish.yaml`](.github/workflows/publish.yaml) 可以在您准备将包发布到 Nuget 时手动调度。

> 要发布到 Nuget，您需要一个名为 `NUGET_API_KEY` 的存储库或组织秘密，其中包含您的 Nuget API 密钥。`NUGET_API_KEY` 必须是 GitHub Actions 秘密以确保其安全！

![发布工作流](docs/publish.png)

### 🏚 Renovatebot

此存储库包含一个 [`renovate.json`](./renovate.json) 配置，用于与 [Renovatebot] 一起使用。Renovatebot 可以在检测到新依赖版本发布时自动打开拉取请求，帮助您保持依赖项的最新状态。由于 Godot 的发布周期非常快，如果您试图保持在 Godot 的最新版本上，自动化依赖更新可以节省大量时间。

![Renovatebot 拉取请求](docs/renovatebot_pr.png)

> 与 Dependabot 不同，Renovatebot 能够将所有依赖更新合并到一个拉取请求中 —— 对于每个子项目都需要相同 Godot.NET.Sdk 版本的 Godot C# 存储库来说，这是必不可少的。如果依赖版本更新被拆分到多个存储库中，构建将在 CI 中失败。

将 Renovatebot 添加到您的存储库的最简单方法是从 [GitHub Marketplace][get-renovatebot] 安装它。请注意，您必须授予它访问您希望它监控的每个组织和存储库的权限。

包含的 `renovate.json` 包含一些配置选项，以限制 Renovatebot 打开拉取请求的频率，以及一些正则表达式来过滤掉一些版本控制不佳的依赖项，以防止无效的依赖版本更新。

---

🐣 包由 🐤 Chickensoft 模板生成 — <https://chickensoft.games>

<!-- 链接 -->

<!-- 页眉 -->
[chickensoft-badge]: https://raw.githubusercontent.com/chickensoft-games/chickensoft_site/main/static/img/badges/chickensoft_badge.svg
[chickensoft-website]: https://chickensoft.games
[discord-badge]: https://raw.githubusercontent.com/chickensoft-games/chickensoft_site/main/static/img/badges/discord_badge.svg
[discord]: https://discord.gg/gSjaPgMmYW
[read-the-docs-badge]: https://raw.githubusercontent.com/chickensoft-games/chickensoft_site/main/static/img/badges/read_the_docs_badge.svg
[docs]: https://chickensoft.games/docs
[line-coverage]: badges/line_coverage.svg
[branch-coverage]: badges/branch_coverage.svg

<!-- 文章 -->
[GoDotTest]: https://github.com/chickensoft-games/go_dot_test
[setup-docs]: https://chickensoft.games/docs/setup
[cspell]: https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker
[Renovatebot]: https://www.mend.io/free-developer-tools/renovate/
[get-renovatebot]: https://github.com/apps/renovate
[godot-test-driver]: https://github.com/derkork/godot-test-driver
[coverlet-issues]: https://github.com/coverlet-coverage/coverlet/issues/1422
[GodotSharp]: https://www.nuget.org/packages/GodotSharp/
[chickensoft-games/setup-godot]: https://github.com/chickensoft-games/setup-godot
