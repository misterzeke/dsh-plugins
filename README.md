# dsh-plugins

我的 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) 插件集合（monorepo）。

每个插件在 `plugins/<name>/` 下独立成包，可以单独安装，不需要 clone 整个仓库。

## 插件

| 插件 | 说明 |
| --- | --- |
| [dsh-deepy-mascot](plugins/dsh-deepy-mascot) | 输入框上沿的宠物小鲸鱼 Deepy（`dsh-claude-style` 的 mascot 配置封装） |

## 安装插件

用 pnpm 的 git 子目录语法（`#path:/...`）直接指定包根：

```bash
# CLI：桌面 App 用 desktop，浏览器形态用 web
dsh plugin --profile desktop add "github:misterzeke/dsh-plugins#path:/plugins/dsh-deepy-mascot"
```

Web GUI：侧栏 **插件** → **添加插件**，填同一个 spec → 安装 → 立即启用 → 重启 DSH。

要求：`dsh >= 0.2.2-rc.1`、`git` 可用、能访问 GitHub。

## 目录结构

```
plugins/
└── dsh-deepy-mascot/        # 包根：package.json + dsh.bundle.patch
    ├── package.json
    ├── cordis.patch.yml
    ├── index.js
    ├── icon.svg
    └── README.md
```

## 新增插件

1. 在 `plugins/` 下新建目录，放一个带 `dsh.bundle.patch` 的 `package.json`；
2. 安装时用 `#path:/plugins/<name>` 定位包根，所以仓库根不要放 `package.json`；
3. 在本文件的插件表里加一行，并在插件自己的 README 里写清安装 spec。
