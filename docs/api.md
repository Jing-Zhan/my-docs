# API 参考

以下接口仅用于文档展示，不提供真实服务。

## 查询文档列表

- 请求方法：GET
- 请求路径：/api/docs
- 功能：返回文档条目列表。

### 请求参数

| 参数 | 位置 | 类型 | 是否必填 | 说明 |
| --- | --- | --- | --- | --- |
| `page` | query | integer | 否 | 页码，从 1 开始，默认 1 |
| `size` | query | integer | 否 | 每页条数，默认 20，最大 100 |
| `keyword` | query | string | 否 | 标题关键字，模糊匹配 |

### 返回示例

```json
{
  "items": [
    {"id": 1, "title": "使用指南"}
  ]
}
```

带分页信息时，返回体形如：

```json
{
  "total": 4,
  "page": 1,
  "size": 20,
  "items": [
    {"id": 1, "title": "我的技术文档", "path": "index.md"},
    {"id": 2, "title": "关于本项目", "path": "about.md"},
    {"id": 3, "title": "使用指南", "path": "guide.md"},
    {"id": 4, "title": "API 参考", "path": "api.md"}
  ]
}
```

### 用 curl 调用（示例）

```bash
curl -s "https://example.com/api/docs?page=1&size=2" \
  -H "Accept: application/json"
```

## 错误码

| 状态码 | 含义 | 触发条件 |
| --- | --- | --- |
| 200 | 成功 | 正常返回列表 |
| 400 | 参数错误 | `size` 超过 100 或不是正整数 |
| 404 | 资源不存在 | 请求了未定义的路径 |
| 500 | 服务内部错误 | 服务端异常 |

错误返回体统一为：

```json
{
  "code": 400,
  "message": "size must be between 1 and 100"
}
```

## 字段约定

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `id` | integer | 条目唯一标识，稳定不变 |
| `title` | string | 页面标题，取自 Markdown 一级标题 |
| `path` | string | 相对 `docs/` 的源文件路径 |

## 说明

本页的接口是文档示例，用于练习“请求方法 + 路径 + 参数表 + 返回示例 + 错误码”这种接口文档写法，仓库中并没有对应的后端程序。站点本身是纯静态页面，由 [使用指南](guide.md) 中的命令构建生成。

回到 [首页](index.md) 或查看 [关于本项目](about.md)。
