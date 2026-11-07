# 使用指南

## 日常维护流程

1. 修改 docs 目录中的 Markdown 文件。
2. 在本地预览并检查页面及链接。
3. 提交修改并推送到 main 分支。
4. 在 Actions 页面查看构建与部署状态。
5. 打开网站检查内容是否更新。

第 3 步之后就不用管了：只要推送成功，构建和部署都在 GitHub 的服务器上完成。如果第 4 步看到红色叉号，展开失败的那一步读日志即可，常见问题在本页最后有对照表。

## 常用命令

- python -m mkdocs serve：本地预览。
- python -m mkdocs build --strict：构建网站并严格检查警告。

放在完整的虚拟环境流程里，是这样一组命令：

```bash
# 1. 创建并激活虚拟环境（Windows PowerShell）
python -m venv .venv
.\.venv\Scripts\Activate.ps1

# 2. 安装依赖
python -m pip install -r requirements.txt

# 3. 严格构建：有警告即视为失败
python -m mkdocs build --strict

# 4. 本地预览，默认地址 http://127.0.0.1:8000/
python -m mkdocs serve
```

不同终端的激活方式不一样，选自己那一种即可：

| 终端 | 激活命令 |
| --- | --- |
| Windows Git Bash | `source .venv/Scripts/activate` |
| Windows PowerShell | `.\.venv\Scripts\Activate.ps1` |
| macOS / Linux | `source .venv/bin/activate` |

## 目录约定

| 路径 | 作用 | 能不能手改 |
| --- | --- | --- |
| `docs/` | 页面源码，`nav` 里的路径相对这里 | 可以，这是主要工作区 |
| `mkdocs.yml` | 站点名、主题、导航、扩展 | 可以，注意 YAML 用空格缩进 |
| `requirements.txt` | 依赖版本范围 | 可以，改完 CI 会重装 |
| `site/` | 构建产物 | 不要手改，也不提交 |
| `report/` | 作业报告与截图 | 可以，但改动不会触发部署 |

## 排错对照表

| 现象 | 常见原因 | 处理办法 |
| --- | --- | --- |
| `mkdocs build --strict` 报 `WARNING - Doc file ... contains a link to ... which is not found` | 页面间链接写错，或链接到了不存在的文件 | 改成正确的相对路径，例如 `[使用指南](guide.md)` |
| `nav` 里的页面报不被包含 | `nav` 写了 `docs/index.md` | `nav` 路径相对于 `docs/`，应写 `index.md` |
| YAML 解析失败 | 用了 Tab 缩进 | 全部改成空格 |
| 工作流显示 `build` 失败 | 依赖装不上或构建有警告 | 展开 `Build documentation` 步骤看最后几行日志 |
| `deploy` 被跳过 | `build` 没成功 | 先修 `build`，`deploy` 依赖它 |
| 页面是旧的 | 浏览器缓存 | 强制刷新或无痕窗口打开 |
| 推送后 Actions 没有运行 | 本次只改了 `report/` 或 `README.md` | 属于预期行为，工作流配置了 `paths` 过滤 |

!!! note "为什么只用 `python -m mkdocs`"
    直接调用 `mkdocs` 依赖 `PATH` 里的脚本路径，在虚拟环境和不同终端下容易对不上；用 `python -m mkdocs` 能保证执行的就是当前解释器装好的那一份，CI 里也一样。

## 相关页面

- 想了解项目结构和技术组成：[关于本项目](about.md)
- 想了解接口描述写法：[API 参考](api.md)
- 回到开始：[首页](index.md)
