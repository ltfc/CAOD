# 中国书画艺术开放数据库（China Art Open Data【CAOD】）

中国书画艺术开放数据服务，简称"CAOD"，是中华珍宝馆（ http://ltfc.net ）提供的开放艺术数据服务，提供历代书法绘画作品的描述信息和图片数据文件下载，为书画AI研究提供训练数据，也可以用于其他和传统艺术相关的科研工作。

### 1. 如何使用 CAOD 数据
CAOD 以 WEB 服务的方式提供数据，数据的接口文件描述请参考：

protobuf定义文件：china_art_opendata_service.proto

或者是

swagger定义 ：china_art_opendata_service.swagger.json


您需要首先获取 APPID 和 APPSEC，请发送邮件到中华珍宝馆支持邮箱( support@ltf.net )，说明您的学校和科研项目，获得批准后，我们会尽快为您创建 APPID 和 APPSEC。

获取到正确的 APPID 和 APPSEC 之后，可以使用 HTTP POST 方法请求我们的数据接口服务(https://api.quanku.art)。


## 数据接口说明：

目前提供两个主要的数据接口：

### 2. 获取数据文件列表
https://api.quanku.art/cag2.ChinaArtOpenDataService/list

接口说明：
本接口用于返回CAOD的数据列表，传入数据页面，返回分页后的数据列表。

参数说明:
```javascript
"cag2ListCAODItemReq": {
  "type": "object",
  "properties": {
    "token": {
      "$ref": "#/definitions/cag2CAODToken",
      "title": "访问令牌"
    },
    "page": {
      "$ref": "#/definitions/cag2CAODDataPage",
      "title": "翻页状态"
    }
  },
  "title": "分页获取作品数据"
}
```
请参考 china_art_opendata_service.swagger.json 中的 cag2ListCAODItemReq 定义。

返回值类型：
正常调用的返回值如下：
```javascript
"cag2ListCAODItemRes": {
  "type": "object",
  "properties": {
    "data": {
      "type": "array",
      "items": {
        "$ref": "#/definitions/cag2CAODItem"
      },
      "title": "分页后的作品数据"
    },
    "total": {
      "type": "integer",
      "format": "int32",
      "title": "作品总数"
    }
  },
  "title": "返回作品列表"
},
```
请参考 china_art_opendata_service.swagger.json 中的 "cag2ListCAODItemRes" 定义。

例子:
```
> curl -X POST "https://api.quanku.art/cag2.ChinaArtOpenDataService/list" -H "Content-Type: application/json" -d '{"token": { "apiKey": "t8nwc4m34yn", "apiSec": "axak2qyorrbkmelcyz2" },"page": { "skip": 0, "limit": 1 }}'

{"data":[{"caodsn":"ZB.H31W0", "age":"清", "name":"瓶菊图", "author":"虚谷", "desc":"", "commentInfo":"", "stampInfo":"", "referenceBook":"", "mediaType":"", "materialType":"", "styleType":"", "size":"", "tags":[], "subjects":[], "technique":[]}], "total":3253}%
```

### 3. 获取图片文件下载路径
https://api.quanku.art/cag2.ChinaArtOpenDataService/getDownloadUrl

接口说明:
本接口用于查询特定图片的下载

参数说明:
```javascript
"cag2GetCAODDownloadUrlReq": {
  "type": "object",
  "properties": {
    "token": {
      "$ref": "#/definitions/cag2CAODToken",
      "title": "访问令牌"
    },
    "caodsn": {
      "type": "string",
      "title": "翻页状态"
    }
  },
  "title": "获取作品下载路径"
}
```
请参考 china_art_opendata_service.swagger.json 中的 cag2GetCAODDownloadUrlReq 定义

返回值类型：
正常调用的返回值如下：
```javascript
"cag2GetCAODDownloadUrlRes": {
  "type": "object",
  "properties": {
    "url": {
      "type": "string",
      "title": "分页后的作品数据"
    }
  },
  "title": "返回作品下载路径"
}
```
请参考 china_art_opendata_service.swagger.json 中的 "cag2ListCAODItemRes" 定义。

例子:
```
> curl -X POST "http://127.0.0.1:8080/cag2.ChinaArtOpenDataService/getDownloadUrl" -H "Content-Type: application/json" -d '{"token": { "apiKey": "t8nwc4m34yn", "apiSec": "axak2qyorrbkmelcyz2" },"caodsn": "ZB.H7CQM"}'

{"url":"http://dl-ac.ltfc.net/picstore/5be3971d8ed7f411e26a6393/80_16.jpg?OSSAccessKeyId=LKJLKJlasd2u89234YEDAabE&Expires=3232831537&Signature=M5TmhHlNZ4fBFQZuekIyabFw2gQ%3D"}%
```


### 4. 下载图片文件
使用通过 /cag2.ChinaArtOpenDataService/getDownloadUrl 获取的下载地址，下载图片文件

例子：
```shell
> wget "http://dl-ac.ltfc.net/picstore/5be3971d8ed7f411e26a6393/80_16.jpg?OSSAccessKeyId=LKJLKJlasd2u89234YEDAabE&Expires=3232831537&Signature=M5TmhHlNZ4fBFQZuekIyabFw2gQ%3D"
```

### 5. 数据规模
CAOD 向研究者提供的是经过我们校订的数据，尽量做到准确详细，截止（2021-03-19），我们已经精订了 3865 幅作品，并且以每月数百幅的速度在增加。尽管如此，精订的数据在我们全部数据中占的比例还是非常低，我们的数据资料已经超过18万幅，所以未来我们会投入大量资金进行数据校订，目标是提供一个最全面，最精确的书画大数据集，服务于书画方向的AI研究者和技术研究者，进而让AI能够更好的服务于艺术行业，非常期待看到您的各种天才的创新应用。

单个团队的力量毕竟有限，如果您愿意贡献一份力量，请不要害羞，积极的和我们联系(support@ltfc.net)吧，期待您的参与。
