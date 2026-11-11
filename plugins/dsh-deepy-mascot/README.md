# DSH Deepy 小鲸鱼

把「输入框上沿的宠物小鲸鱼 Deepy」抽成可安装的 DSH 组合包（bundle），是 [dsh-plugins](../../) 仓库的第一个模块。

> 鲸鱼本身不是独立插件 —— 它是主题插件 [dsh-claude-style](https://github.com/Nwflower/dsh-claude-style) 的 `mascot` 功能（`mascot: deepy`）。本包只做两件事：把该插件作为依赖装上，并把原机器 profile patch 里的 `ui-skin-claude-style` 配置原样写进 patch。

## 文件说明

| 文件 | 作用 |
| --- | --- |
| `package.json` | 组合包清单：声明 bundle patch 路径，并把 `dsh-claude-style@0.20.2` 作为依赖 |
| `cordis.patch.yml` | 真正生效的配置：插入 `ui-skin-claude-style` 行并固定成你的当前配置（本包独立安装时用这份） |
| `overlay.cordis.patch.yml` | 备用配置：目标机已经单独启用了 `dsh-claude-style` 时，用这份「按 id 覆盖」的条目，避免重复插入 |
| `index.js` | 组合包入口（空模块，仅供包解析） |
| `icon.svg` | 插件列表里的图标 |

## 安装

先决条件：目标机是 **Web GUI** 形态的 DSH（桌面 App 的 `desktop` profile 或浏览器的 `web` profile），版本 `>= 0.2.2-rc.1`，能访问 GitHub，并且 **git 可用**（`git --version` 能跑；用 GitHub spec 安装时 pnpm 和 DSH 的安装前检查都依赖它）。没有 git 就走方式 C 或本地目录安装。

### 方式 A：插件页（推荐）

1. 打开侧栏 **插件** → **添加插件**
2. 输入 `github:misterzeke/dsh-plugins#path:/plugins/dsh-deepy-mascot`，点 **安装**
3. 装完点 **立即启用**（安装第三方组合包默认也会自动启用）
4. 重启 DeepSeek Harness，回到对话页即可看到鲸鱼

### 方式 B：命令行

```bash
# 桌面 App 用 desktop；浏览器 / web 形态用 web
dsh plugin --profile desktop add "github:misterzeke/dsh-plugins#path:/plugins/dsh-deepy-mascot"
```

`#path:/plugins/dsh-deepy-mascot` 是 pnpm 的 git 子目录语法，指定 monorepo 里的包根，所以不用 clone 整个仓库。

### 方式 C：手动（兜底，GitHub 不通时）

1. 在插件页安装并启用 `dsh-claude-style@0.20.2`（它自己会插入 `ui-skin-claude-style` 行）
2. 把 `overlay.cordis.patch.yml` 的条目**追加**到
   `<DSH_HOME>/profiles/<profile>/cordis.patch.yml`
   （注意：此时不要用 `cordis.patch.yml` 里的 `- insert:`，否则同一个 id 会被插入两次）
3. 重启 DSH

也可以把 `dsh-plugins` clone 到本地，用插件页「添加插件」填 `plugins/dsh-deepy-mascot` 的绝对路径安装（同样走独立包流程，用 `cordis.patch.yml`）。

## 配置项

`cordis.patch.yml` 里的 `config` 完全对应 [dsh-claude-style 的设置](https://github.com/Nwflower/dsh-claude-style)：

| 键 | 当前值 | 含义 | 可选值 |
| --- | --- | --- | --- |
| `mascot` | `deepy` | **输入框上沿的宠物** | `deepy`（小鲸鱼）/ `brand`（跟随品牌）/ `crab` / `whalegirl` / `off` |
| `mascotScope` | `all` | 宠物出现范围 | `all`（首页 + 对话页）/ `home`（仅首页） |
| `brand` | `deepseek` | 品牌形象 | `deepseek` / `claude` |
| `palette` | `host` | 配色来源 | `host`（跟随宿主，保持 DSH 原生观感）/ `claude` |
| `typeface` | `host` | 字体来源 | `host` / `claude` / `system` |
| `sidebarBrand` | `true` | 侧栏是否绘制品牌区 | `true` / `false` |
| `sodium` | `on` | 钠渲染引擎 | `on` / `off` / `undecided` |
| `banLocale` | `en` | 界面语言限制 | `en` / `zh` / `off` |

### 只想要鲸鱼、不要主题配色

把 `config` 精简成下面两行即可，其余键回到插件默认值（Claude 品牌配色）：

```yaml
      config:
        mascot: deepy
        mascotScope: all
```

## 修改与升级

- 换鲸鱼以外的东西：直接改 `cordis.patch.yml` 的 `config`，重装/重启生效。
- 升级主题：改 `package.json` 里 `"dsh-claude-style": "0.20.2"` 的版本号，重新安装。
- 想跟随最新版而不是钉死版本：改成 `"^0.20.2"`（复现效果可能漂移）。

## 卸载

- 插件页里停用或移除 `dsh-deepy-mascot`；
- 或从 `<DSH_HOME>/profiles/<profile>/package.json` 删除依赖、从 `dsh.profile.bundles` 删除条目后重启。

停用后鲸鱼消失，`dsh-claude-style` 依赖可以保留（不启用就不会加载）。

## 注意

- **只在 Web GUI 生效**：`dsh-claude-style` 是 web 客户端插件，headless / TUI profile 不会有鲸鱼。
- **不要同时把 `dsh-claude-style` 作为组合包启用**：本包已经插入了同一个 `ui-skin-claude-style` 行，两条 insert 会撞 id。要用这个仓库就只启用本包。
- 本包不包含 `dsh-claude-style` 的任何代码或素材，只在依赖里引用它；主题版权归其作者 Nwflower（MIT）。
- 钉 `0.20.2` 是因为这就是原机器当前生效的版本；换版本前先确认效果。

## 所属仓库

本插件是 [misterzeke/dsh-plugins](https://github.com/misterzeke/dsh-plugins) 的一个模块：仓库根只放索引，每个插件在 `plugins/<name>/` 下，各自独立成包。
