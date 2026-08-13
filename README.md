# 兔兔 - AstrBot JM 漫画下载插件

通过 JM 禁漫本子编号下载漫画，打包为压缩包发送给你，支持 **Agent 自动判断触发**。

> 插件 id：`jm_downloader`　当前版本：`1.0.0`

## 触发方式

本插件注册了 LLM 函数工具 `download_jm_comic`，**Agent 会根据对话语义自动调用**，无需固定指令。例如：

- 「帮我下载 JM350234」
- 「下载本子 350234」
- 「找本 350234」

命令回退（需 @ 机器人或唤醒词）：

```
找本 [编号]
```

- 例：`找本 350234`
- 例：`找本350234`（中间无空格也可以）

## 功能说明

1. 解析消息中的数字编号
2. 调用 `jmcomic` 库下载对应本子所有图片
3. 下载完成后自动打包为 zip（**文件名仅为本子编号**，如 `350234.zip`，避免标题过长导致发送失败）
4. 通过消息平台将压缩包作为文件发送给你
5. 下载文件保存在 `data/download/jmcomic/` 目录
6. 可在管理面板配置是否给压缩包加密；开启且未自定义密码时，默认用本子编号作密码
7. 下载是后台任务，同一编号进行中不会重复下载；文件发送失败自动重试 3 次

## 管理面板配置

安装后在 AstrBot 管理面板的插件配置中修改：

| 配置项 | 说明 | 默认 |
| --- | --- | --- |
| `jm_enabled` | 插件总开关 | `true` |
| `jm_zip_password_enabled` | 是否给导出 zip 添加密码 | `false` |
| `jm_zip_password` | 自定义密码；留空且开启加密时，使用当前本子编号 | 空 |

开启加密后，下载完成的提示里会附带密码。

## 依赖

- Python 包：`jmcomic`（见 `requirements.txt`，安装插件时自动安装）

> 旧版 AmiyaBot 插件内还打包了 `pyzipper`、`Cryptodome` 用于加密；jmcomic 已内置 zip 加密导出，无需额外安装。

## 安装方法

1. 执行打包脚本，生成 `plugins/astrbot/dist/siwu-jm-downloader-<版本号>.zip`
2. 打开 AstrBot 管理面板 → 插件管理 → 安装插件 → 上传该 zip
3. AstrBot 自动解压到 `data/plugins/`、安装依赖并加载插件

### 重新打包

```bash
python plugins/astrbot/siwu-jm-downloader-1_0/build.py
```

## 版本记录

每次发版在表格最上方追加一行。

| 版本 | 更新内容 |
|---|---|
| `1.0.0` | 迁移至 AstrBot：注册 Agent 函数工具 `download_jm_comic` 自动触发；后台下载避免工具超时；「找本」命令回退；支持压缩包加密 |

## 项目地址

<https://github.com/siwuli/starbot_siwu-jm-downloader>
