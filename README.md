# 中国艺术开放数据服务

## 关于CAOD（China Art Open Data）
中国艺术开放数据服务，简称"CAOD"，是由[中华珍宝馆](http://ltfc.net)维护的免费开放数据服务，提供最全的中国传统艺术数据，内容主要是历代书法、绘画作品信息查询、图片文件下载。

CAOD可以为中国艺术方向的AI研究提供训练数据，也可以用于其他和传统艺术相关的科研工作，我们遵循CC开放协议，所提供的数据和图片可以任意使用，无需付费，无需注明出处。

## 数据接口 (1.0 Beta)
### 1. 如何使用 CAOD 数据
CAOD 以 WEB 服务的方式提供数据，数据的接口文件描述有下面两种，您可以选择其中一个作为参考：
* protobuf定义文件：china_art_opendata_service.proto
* swagger定义 ：china_art_opendata_service.swagger.json


在调用服务接口之前，您需要首先获取 APPID 和 APPSEC，请发送邮件到中华珍宝馆支持邮箱( support@ltfc.net )，说明您的学校和科研项目，获得批准后，我们会尽快为您创建 APPID 和 APPSEC。

获取到正确的 APPID 和 APPSEC 之后，可以使用 HTTP POST 方法请求我们的数据接口服务(https://api.quanku.art)，关于接口下文有更详细的描述。

### 2. 获取数据文件列表
https://api.quanku.art/cag2.ChinaArtOpenDataService/list

#### 接口说明：
本接口用于返回CAOD的数据列表，传入数据页面，返回分页后的数据列表。

#### 参数说明
请求参数的数据格式如下：
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
      "title": "分页状态"
    }
  },
  "title": "分页获取作品数据"
}
```
请参考 china_art_opendata_service.swagger.json 中的 cag2ListCAODItemReq 定义。

#### 返回值类型
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


#### 调用例子:
```bash
> curl -X POST "https://api.quanku.art/cag2.ChinaArtOpenDataService/list" -H "Content-Type: application/json" -d '{"token": { "apiKey": "t8nwc4m34yn", "apiSec": "axak2qyorrbkmelcyz2" },"page": { "skip": 0, "limit": 1 }}'

{"data":[{"caodsn":"ZB.H31W0", "age":"清", "name":"瓶菊图", "author":"虚谷", "desc":"", "commentInfo":"", "stampInfo":"", "referenceBook":"", "mediaType":"", "materialType":"", "styleType":"", "size":"", "tags":[], "subjects":[], "technique":[]}], "total":3253}%
```

### 3. 获取图片文件下载路径
https://api.quanku.art/cag2.ChinaArtOpenDataService/getDownloadUrl

#### 接口说明:
本接口用于查询特定图片的下载

#### 参数说明
调用参数格式如下：
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

#### 返回值类型：
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

{"url":"http://dl-ac.ltfc.net/picstore/5be3971d8ed7f411e26a6393/80_16.jpg?OSSAccessKeyId=XXXXXXXXXXX&Expires=3232831537&Signature=XXXXXXXXXXXX"}%
```

### 4. 下载图片文件
使用通过 /cag2.ChinaArtOpenDataService/getDownloadUrl 获取的下载地址，下载图片文件

例子：
```bash
> wget "http://dl-ac.ltfc.net/picstore/5be3971d8ed7f411e26a6393/80_16.jpg?OSSAccessKeyId=XXXXXXXXXXXX&Expires=3233447046&Signature=XXXXXXXXXXXX" -O test.jpg

--2021-03-26 09:51:48--  http://dl-ac.ltfc.net/picstore/5be3971d8ed7f411e26a6393/80_16.jpg?OSSAccessKeyId=XXXXXXXXXXXX&Expires=3233447046&Signature=cF%XXXXXXXXXXXX%3D
Resolving dl-ac.ltfc.net (dl-ac.ltfc.net)... 222.222.88.78
Connecting to dl-ac.ltfc.net (dl-ac.ltfc.net)|222.222.88.78|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 170090 (166K) [image/jpeg]
Saving to: ‘test.jpg’

test.jpg                                                             100%[====================================================>] 166.10K  --.-KB/s    in 0.03s

2021-03-26 09:51:48 (5.48 MB/s) - ‘test.jpg’ saved [170090/170090]
```
### 5. 调用频次限制
对接口有调用频次限制，每分钟调用次数<500次，日总计调用次数<100000 次，请控制您的调用频率。

### 6. 数据规模
CAOD 向研究者提供的是经过我们校订的数据，尽量做到准确详细，截止到 2021-03-19，我们已经精订了 3865 幅作品，并且以每月数百幅的速度在增加。

尽管如此，精订的数据在我们全部18万多的数据中占的比例还是非常低，未来我们会投入大量资金进行数据校订，目标是提供一个最全面，最精确的书画大数据集，服务于书画方向的AI研究者和技术研究者，进而让AI能够更好的服务于艺术行业，非常期待看到您的各种天才的创新应用。

单个团队的力量毕竟有限，如果您愿意贡献一份力量，请不要害羞，积极的和我们联系(support@ltfc.net)吧，期待您的参与。

### 7. 已合作高校和机构
* _中国科技大学_
* _香港中文大学（深圳）_
