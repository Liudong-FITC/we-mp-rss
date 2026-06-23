# API 接口文档

基础 URL: `http://192.168.100.230:8003/api/v1/wx`

认证方式: 请求头添加 `Authorization: AK-SK {AK}:{SK}`

---

## 1. 获取文章列表

### 请求

**`GET /articles`**

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| offset | int | 否 | 0 | 偏移量，从第几条开始 |
| limit | int | 否 | 5 | 每页数量，最大 100 |
| status | string | 否 | - | 文章状态筛选，多个用逗号分隔（如 `1,6`） |
| search | string | 否 | - | 关键词搜索（标题/描述） |
| mp_id | string | 否 | - | 公众号ID筛选 |
| only_favorite | bool | 否 | false | 仅返回收藏的文章 |
| has_content | bool | 否 | - | 是否有正文（true=有，false=无，不传=全部） |

### 响应

```json
{
  "code": 0,
  "message": "success",
  "data": {
    "list": [
      {
        "id": "string",
        "mp_id": "string",
        "mp_name": "string",
        "title": "string",
        "pic_url": "string",
        "url": "string",
        "description": "string",
        "content": "string",
        "content_html": "string",
        "status": 1,
        "publish_time": 1781064670,
        "create_time": 1781064670,
        "publish_type": 0,
        "publish_src": 1,
        "publish_status": "200",
        "publish_info": "string (JSON)",
        "original_check_type": 0,
        "in_profile": 0,
        "pre_publish_status": 0,
        "service_type": 0,
        "show_type": 0,
        "item_show_type": 0,
        "copyright_stat": 0,
        "has_red_packet_cover": 0,
        "art_type": 0,
        "extinfo": "string",
        "created_at": "2026-06-10T12:14:02",
        "updated_at": 1781064842,
        "updated_at_millis": 1781064842661,
        "is_export": null,
        "is_read": 0,
        "is_favorite": 0,
        "fix_fail_count": 0,
        "has_content": 1
      }
    ],
    "total": 66
  }
}
```

### 字段说明

| 字段 | 类型 | 说明 |
|------|------|------|
| id | string | 文章唯一ID |
| mp_id | string | 公众号ID |
| mp_name | string | 公众号名称 |
| title | string | 文章标题 |
| pic_url | string | 封面图片URL |
| url | string | 文章链接 |
| description | string | 文章摘要 |
| content | string | 正文（纯文本） |
| content_html | string | 正文（HTML） |
| status | int | 1=正常, 6=抓取中, 1000=已删除 |
| publish_time | int | 发布时间（Unix时间戳） |
| create_time | int | 创建时间（Unix时间戳） |
| is_read | int | 是否已读（0/1） |
| is_favorite | int | 是否收藏（0/1） |
| has_content | int | 是否有正文（0/1） |

### 示例

```bash
curl "http://192.168.100.230:8003/api/v1/wx/articles?offset=0&limit=10&mp_id=MP_WXS_3236757533" \
  -H "Authorization: AK-SK WKxxx:SKxxx"
```

---

## 2. 获取公众号列表

### 请求

**`GET /mps`**

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| page | int | 否 | 1 | 页码 |
| page_size | int | 否 | 10 | 每页数量，最大 100 |
| kw | string | 否 | - | 关键词搜索（名称/简介） |
| status | int | 否 | - | 状态筛选（1=启用, 0=停用） |

### 响应

```json
{
  "code": 0,
  "message": "success",
  "data": {
    "list": [
      {
        "id": "string",
        "mp_name": "string",
        "mp_cover": "string",
        "mp_intro": "string",
        "status": 1,
        "faker_id": "string",
        "sync_time": 1781064842,
        "update_time": 1781064842
      }
    ],
    "total": 1,
    "page": 1,
    "size": 10
  }
}
```

### 字段说明

| 字段 | 类型 | 说明 |
|------|------|------|
| id | string | 公众号唯一ID |
| mp_name | string | 公众号名称 |
| mp_cover | string | 公众号头像URL |
| mp_intro | string | 公众号简介 |
| status | int | 1=启用, 0=停用 |
| faker_id | string | 关联的浏览器会话ID |
| sync_time | int | 最后同步时间（时间戳） |
| update_time | int | 更新时间（时间戳） |

### 示例

```bash
curl "http://192.168.100.230:8003/api/v1/wx/mps?page=1&page_size=10&status=1" \
  -H "Authorization: AK-SK WKxxx:SKxxx"
```

---

## 3. 获取标签列表

### 请求

**`GET /tags`**

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| offset | int | 否 | 0 | 偏移量 |
| limit | int | 否 | 100 | 每页数量 |

### 响应

```json
{
  "code": 0,
  "message": "success",
  "data": {
    "list": [
      {
        "id": "string",
        "name": "string",
        "cover": "string",
        "intro": "string",
        "status": 1,
        "mps_id": "string",
        "sync_time": 1781064842,
        "update_time": 1781064842,
        "created_at": "2026-06-10T12:14:02",
        "updated_at": "2026-06-10T12:14:02"
      }
    ],
    "page": {
      "limit": 10,
      "offset": 0,
      "total": 5
    },
    "total": 5
  }
}
```

### 字段说明

| 字段 | 类型 | 说明 |
|------|------|------|
| id | string | 标签唯一ID |
| name | string | 标签名称 |
| cover | string | 标签封面URL |
| intro | string | 标签简介 |
| status | int | 1=启用, 0=停用 |
| mps_id | string | 关联的公众号ID集合（JSON格式） |
| sync_time | int | 最后同步时间（时间戳） |
| update_time | int | 更新时间（时间戳） |
| created_at | string | 创建时间（ISO 8601） |
| updated_at | string | 更新时间（ISO 8601） |

### 示例

```bash
curl "http://192.168.100.230:8003/api/v1/wx/tags?offset=0&limit=10" \
  -H "Authorization: AK-SK WKxxx:SKxxx"
```

---

## 通用错误响应

```json
{
  "code": 50001,
  "message": "错误描述"
}
```

| 错误码 | 说明 |
|--------|------|
| 0 | 成功 |
| 401 | 认证失败 |
| 404 | 资源不存在 |
| 500 | 服务器内部错误 |
| 50001 | 业务错误 |
