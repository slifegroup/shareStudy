### 查询条件使用类似`like` 

```sql
db.videos.find({"name":/^仁心解码/}) // 类似 like 仁心解码%

db.videos.find({"name":/仁心解码/}) // 类似 like %仁心解码%

db.videos.find({"name":/仁心解码$/}) // 类似 like %仁心解码
```

其实就是用到了正则匹配

### 使用 `python` 获取集合的条数

* 使用 count_documents

```
self.db.videos.count_documents({"name": name})
```