# indexedDB

javascript promise-based indexedDB

## 简介

基本的增(add)删(delete)改(put)查(get)的功能

不用关心IndexedDB的version问题.

## 使用

默认会创建有一个配置库,用来管理所有的database的version.

首次创建数据库是会默认创建配置库,也可调用initialize手动创建,但是不推荐使用.

配置库名:"aidb_default",有一张配置表:"aidb_params".

会保存所有使用aidb创建的数据库,用来管理数据库的version.而不用手动管理.

如果在使用aidb前就创建过数据库,可以直接在配置表中新增记录,加入到aidb的管理

## createTable

```javascript

aidb().open("DB_TestDB").createTable("mainTable",{keyPath: 'id'},{key:"subId",unique:true}).execude().then(()=>{
    console.log("success")
}).catch(()=>{
    console.log("error")
})

```

open(database,version) 打开数据库,如果不存在会自动创建.接受两个字段database,version.一般不建议传入version,数据库version会自动从配置里读取.不用手动维护了

createTable(tablename,keypath,index)接受三个参数,1.表名,2.主键,3.索引({key:索引字段,unique:是否唯一})

所有的操作只有在执行execude()之后才会保存到数据库.并且返回一个Promise对象.

## add

新增单个

```javascript
    aidb().open("DB_TestDB").add("mainTable",
        {
            id:"1",
            subId:"1",
            params:{
                param1:"params1",
                param2:"params2",
                },
            arr:[1,2,3,4]
        }).execude()

```

新增多个

```javascript
aidb().open("DB_TestDB").add("mainTable",
    [{
        id:"2",
        subId:"2",
        params:{
            param1:"params1",
            param2:"params2",
        },
        arr:[1,2,3,4]
    },
    {
        id:"3",
        subId:"3",
        params:{
            param1:"params1",
            param2:"params2",
        },
        arr:[1,2,3,4]
    }
]).execude()
```

也可以分开写

```javascript
aidb().open("DB_TestDB").add("mainTable",
    {
        id:"2",
        subId:"2",
        params:{
            param1:"params1",
            param2:"params2",
        },
        arr:[1,2,3,4]
    }
).add("mainTable",
    {
        id:"3",
        subId:"3",
        params:{
            param1:"params1",
            param2:"params2",
        },
        arr:[1,2,3,4]
    }
).execude()

```

## put

aidb().open("DB_TestDB").put("mainTable",
    {
        id:"2",
        subId:"2",
        params:{
            param1:"params1",
            param2:"params2",
        },
        arr:[1,2,3,4].reverse();
    }
).execude()

## delete

aidb().open("DB_TestDB").delete("mainTable",
    {
        id:"2",
        subId:"2",
        params:{
            param1:"params1",
            param2:"params2",
        },
        arr:[1,2,3,4].reverse();
    }
).execude()

## get

获取单个

```JavaScript
aidb().open("DB_TestDB").get("mainTable",1).then((result)={
    console.log(result)
})
```

获取多个

```JavaScript
aidb().open("DB_TestDB").get("mainTable",[1,2,4,5]).then((result)={
    console.log(result)
})
```

带条件查询
获取subId字段为1,2,3,4的值,等同于getQuery("mainTable",{"subId":[1,2,4,5]})

```javascript
aidb().open("DB_TestDB").get("mainTable",{"subId":[1,2,4,5]}).then((result)={
    console.log(result)
})

```

索引查询,区别于上一个语句.由于IndexedDB的api局限性.subId是单一的值,而不是一个数组.且只传入单一的index.

```JavaScript
aidb().open("DB_TestDB").get("mainTable",{"subId":1}).then((result)={
    console.log(result)
})

```

多条件查询
获取id为1且subId字段为1,2,3,4的值等同于getQuery("mainTable",{"id":"1","subId":[1,2,4,5]})

```javascript
aidb().open("DB_TestDB").get("mainTable",{"id":"1","subId":[1,2,4,5]}).then((result)={
    console.log(result)
})

```

条件查询(可查询非索引字段.开销较大)

```JavaScript
aidb().open("DB_TestDB").getQuery("mainTable",{ params:{
            param1:"params1",
            param2:"params2",
        },"subId":[1,2,4,5]}).then((result)={
    console.log(result)
})
```

获取全部

```JavaScript
aidb().open("DB_TestDB").getAll("mainTable").then((result)={
    console.log(result)
})

```
