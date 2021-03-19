# 中国书画艺术开放数据库（China Art Open Data【CAOD】）
中国书画艺术开放数据服务，简称"CAOD"，是由中华珍宝馆（ http://ltfc.net ）提供的一个免费开放的数据服务，提供历代书法绘画作品的描述信息和图片数据文件下载，为书画AI研究提供训练数据，也可以用于其他和传统艺术相关的科研工作。

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
//TODO：添加接口详细使用说明

### 3. 获取图片文件下载路径
https://api.quanku.art/cag2.ChinaArtOpenDataService/getDownloadUrl
//TODO：添加接口详细使用说明

### 4. 下载图片文件
使用通过 /cag2.ChinaArtOpenDataService/getDownloadUrl 获取的下载地址，下载图片文件
//TODO：添加接口详细使用说明

### 5. 数据规模
CAOD 向研究者提供的是经过我们校订的数据，尽量做到准确详细，截止（2021-03-19），我们已经精订了 3865 幅作品，并且以每月数百幅的速度在增加。尽管如此，精订的数据在我们全部数据中占的比例还是非常低，我们的数据资料已经超过18万幅，所以未来我们会投入大量资金进行数据校订，目标是提供一个最全面，最精确的书画大数据集，服务于书画方向的AI研究者和技术研究者，进而让AI能够更好的服务于艺术行业，非常期待看到您的各种天才的创新应用。

单个团队的力量毕竟有限，如果您愿意贡献一份力量，请不要害羞，积极的和我们联系吧，期待您的参与。
