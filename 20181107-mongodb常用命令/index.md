# MongoDB常用操作命令

```python3
import pymongo

myclient = pymongo.MongoClient(host="localhost", port=27017)
mydb = myclient.database_name
collection_obj = mydb.collection_name
# upsert 是否插入新数据，如果不存在则插入，存在则更新 
collection_obj.update_one({"id": "1"}, {"$set": {"id": "1", "age": 18}}, upsert=True)
```

### 统计 collection信息 

`db.getCollection('collection_name').stats()`

