## json-server
### 环境搭建
- 下载json-server： `npm install -g json-server`
- 启动： `json-server --watch db.json`
- 修改端口号： `json-server --watch db.json --port 5000(要修改的端口号)`
- 添加web测试环境：`npm init vite@latest`



### GET方法

查询所有`http://localhost:3000/car`

```json
[
  {
    "id": 1,
    "name": "奔驰"
  },
  {
    "id": 2,
    "name": "宝马"
  },
  {
    "id": 3,
    "name": "奥迪"
  }
]
```

**查询某类汽车**

`http://localhost:3000/car/3`	返回的是对象

```json
{
  "id": 3,
  "name": "奥迪"
}
```

`http://localhost:3000/car?id=3` 返回的是数组 根据业务需求选择

```json
[
  {
    "id": 3,
    "name": "奥迪"
  }
]
```

`http://localhost:3000/car?name=奔驰`

```json
[
  {
    "id": 1,
    "name": "奔驰"
  }
]
```

**全局模糊搜索**

查询全局带有关键字的数据 ==> 使用`?q=模糊搜索内容`

`http://localhost:3000/car?q=奔驰`	全局在car下搜索带奔驰的内容

```json
[
  {
    "id": 1,
    "name": "奔驰"
  },
  {
    "id": 4,
    "name": "奔驰"
  },
  {
    "id": 7,
    "name": "奔驰"
  }
]
```

**分页查询**

`_page`：第几页，`_limit`：每页多少条

第1页，每页2条内容 `http://localhost:3000/car?_page=1&_limit=2` 

```json
[
  {
    "id": 1,
    "name": "奔驰"
  },
  {
    "id": 2,
    "name": "宝马"
  }
]
```

第2页，每页5条内容  `http://localhost:3000/car?_page=2&_limit=5`

```json
[
  {
    "id": 6,
    "name": "奥迪"
  },
  {
    "id": 7,
    "name": "奔驰"
  },
  {
    "id": 8,
    "name": "宝马"
  },
  {
    "id": 9,
    "name": "奥迪"
  }
]
```

**分页加排序**

按什么字段排序：`_sort`,排序方式：`desc`降序，`asc`升序

分页后按id升序排`http://localhost:3000/car?_page=2&_limit=5&_sort=id&_order=asc`

```json
[
  {
    "id": 6,
    "name": "奥迪"
  },
  {
    "id": 7,
    "name": "奔驰"
  },
  {
    "id": 8,
    "name": "宝马"
  },
  {
    "id": 9,
    "name": "奥迪"
  }
]
```

**截取**

从第1个开始截取（不包含第1个）截取到第3个(包含第3个)：`http://localhost:3000/car?_start=1&_end=3`

可以简单理解为：`包尾不包头`

```json
[
  {
    "id": 2,
    "name": "宝马"
  },
  {
    "id": 3,
    "name": "奥迪"
  }
]
```

从第1个开始截取（不包括第1个）往后面截取3个数据 `http://localhost:3000/car?_start=1&_limit=3`

```json
[
  {
    "id": 2,
    "name": "宝马"
  },
  {
    "id": 3,
    "name": "奥迪"
  },
  {
    "id": 4,
    "name": "奔驰"
  }
]
```

 **范围过滤条件**

`_gte`大于，`_lte`小于。用于设置一个取值范围

如选择id在5-8之间到数据:`http://localhost:3000/car?id_gte=5&id_lte=8`

```json
[
  {
    "id": 5,
    "name": "宝马"
  },
  {
    "id": 6,
    "name": "奥迪"
  },
  {
    "id": 7,
    "name": "奔驰"
  },
  {
    "id": 8,
    "name": "宝马"
  }
]
```

**可以使用`_ne`剔除某个数据**

如剔除id为5和8的数据，剔除后也可以做分页，这里不再做演示：`http://localhost:3000/car?id_ne=5&id_ne=8`

```json
[
  {
    "id": 1,
    "name": "奔驰"
  },
  {
    "id": 2,
    "name": "宝马"
  },
  {
    "id": 3,
    "name": "奥迪"
  },
  {
    "id": 4,
    "name": "奔驰"
  },
  {
    "id": 6,
    "name": "奥迪"
  },
  {
    "id": 7,
    "name": "奔驰"
  },
  {
    "id": 9,
    "name": "奥迪"
  }
]
```

**局部模糊搜索**

使用`_like` ,如查询字段`name`带有“奔”子的数据，==> `name_like=奔`

`http://localhost:3000/car?name_like=奔`

```json
[
  {
    "id": 1,
    "name": "奔驰"
  },
  {
    "id": 4,
    "name": "奔驰"
  },
  {
    "id": 7,
    "name": "奔驰"
  },
  {
    "id": 10,
    "name": "奔奔"
  }
]
```

**实现类似多表查询**

数据准备: `animal`中的`id`和`skills`中的`animalId`一样对应

```json
{
  "animal":[
    {
      "id":1,
      "name":"兔子"
    },
    {
      "id":2,
      "name":"猴子"
    },
    {
      "id":3,
      "name":"狗子"
    }
  ],
  "skills":[
    {
      "id":1,
      "body":"兔子吃萝卜",
      "animalId":1
    },
    {
      "id":3,
      "body":"狗子汪汪叫",
      "animalId":3
    },
    {
      "id":2,
      "body":"猴子吃香蕉",
      "animalId":2
    }
  ]
}
```

embed因为意思就说嵌入，在animal表数据中嵌入skills表数据，通过animal中的id和skills中的animalId作关联：`http://localhost:3000/animal?_embed=skills`

```json
[
  {
    "id": 1,
    "name": "兔子",
    "skills": [
      {
        "id": 1,
        "body": "兔子吃萝卜",
        "animalId": 1
      }
    ]
  },
  {
    "id": 2,
    "name": "猴子",
    "skills": [
      {
        "id": 2,
        "body": "猴子吃香蕉",
        "animalId": 2
      }
    ]
  },
  {
    "id": 3,
    "name": "狗子",
    "skills": [
      {
        "id": 3,
        "body": "狗子汪汪叫",
        "animalId": 3
      }
    ]
  }
]
```

### POST方法

post添加的数据会直接加入db.json里面，id可以不写，是自增的。

```js
 btn.addEventListener("click", function() {
   axios.post("http://localhost:3000/car", {
     "name": "雷克萨斯",
   }).then(res => {
     console.log(res.data);
   })
 })
```

### PUT方法

修改skills数据，如果出入body和animalId则覆盖原来两个字端的内容，如果只传body的修改，animalId会被删掉，反之亦然。

**修改一个字段**

```js
 btn.addEventListener("click", function() {
   axios.put("http://localhost:3000/skills", {
     "body": "呵呵",
   }).then(res => {
     console.log(res.data);
   })
 })
```

```json
// 使用put只修改原来的body字段，发现animalId不见了，其实是body修改覆盖了原来的body和animalId
{
      "body": "呵呵",
      "id": 1
 },
```

**修改全部字段**

```js
 btn.addEventListener("click", function() {
   axios.put("http://localhost:3000/skills", {
     "body": "呵呵",
     "animalId":1
   }).then(res => {
     console.log(res.data);
   })
 })
```

```json
// 修改的body和animalId把原来的body和animalId进行覆盖
{
      "body": "呵呵",
      "animalId": "1",
      "id": 1
},
```

### PATCH方法

patch方法和put方法不一样，patch值修改`body`不会把`animalId`删除掉

```js
 btn.addEventListener("click", function() {
   axios.patch("http://localhost:3000/skills", {
     "body": "小兔子爱吃萝卜",
   }).then(res => {
     console.log(res.data);
   })
 })
```

```json
 {
      "body": "小兔子爱吃萝卜",
      "animalId": "1",
      "id": 1
 },
```

### DELETE方法

```js
btn.addEventListener("click", function() {
  axios.delete("http://localhost:3000/skills/1").then(res => {
    console.log(res.data);
  })
})
```

