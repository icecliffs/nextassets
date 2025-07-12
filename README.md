# NextAssets (全球隼)

> 下一代资产规划，致力于打造全网最烂的、BUG最多、功能最少、并且资产最少、扫描与攻击面基本没有的攻击面平台

---

<div align="center">  
    <img src="./assets/不安です.png"> 
</div> 

## 简介

**NextAssets** 是一款面向赛博空间保安从业者与互联网情报搜集保安的自动化资产发现与整理平台，支持多维度的网络空间测绘采集与多维度资产管理，覆盖域名/IP/端口等基础信息，支持HTTP/S、安全扫描、漏洞扫描、组件识别、指纹识别、自动化FUZZ、JS安全分析、SSL证书分析等功能，便于高效网络空间测绘与威胁关联分析，本系统会自动化对扫描到的数据进行数据清洗和去重，并且可以针对 SRC 项目进行自动化监控。

> PS：如果您觉得该项目眼熟，那就对了！如有巧合，纯属雷同。

在此之前，曾使用Python写过一个非常垃圾的版本：http://github.com/icecliffs/Cliffscan

<div align="center">
<p>本项目所使用的技术仅供安全人员使用，出了事情后果自负，本项目仅供学习交流</p>
</div>

## 功能

- [x] 网站结构解析与 JS 路径提取、多维度 XSS 扫描（基于 DOM 技术，无头实时编译网站并捕获 XSS 信息）
- [x] 数据可视化支持（可拓展）、端口 / 协议 / 中间件识别，使用 DeepSeek 分析请求数据，自动化提取敏感信息和风险信息
- [x] 资产多维度识别（域名 / IP / 组件 / 指纹）、多维资产自动关联（如：关联 `bilibili.com` 相关 IP）
- [x] SSL 证书关联与证书信息提取、网站截图与 HTTP 响应分类、语法搜索功能、分布式扫描

- [x] IPv6 支持、扫描器认证机制、蜜罐 / 蜜网识别

- [ ] 全球资产测绘进度（娱乐用）<img src="https://img.shields.io/badge/%E8%BF%9B%E5%BA%A6-0.00594%25(222000/3706585103)-brightgreen)"/>，自2025年7月9日以来

## 安装

1. 首先到 Release 下载扫描器  `nextassets_scanner-linux_x86_64.zip`
2. ~~接着配置 nextassets 扫描器里的 `config.yaml`，需要到 [Next Assets 授权中心](https://nextassets.iloli.moe) 进行一机一码认证授权，将授权码贴到配置文件里~~，不需要授权了，我懒得写，爱咋扫咋扫吧，只需要配置mysql就行

```yaml
verify: BMzTJmezAzPWn9/T9BpMuHnDn336PCPRMFAq9wpuH778ouYZeOCsmk8SYxOZ5O8K6sHjNo/jPV1DX4vt5eWTQiSaXAxWtE2tib9TY9DJxanNOGIptVD7yyEenKSbvDsH4FSyzCGWcTnc1AGKJrAVA478XCaFfNRIiPYJtOXTqSwhtDIACX2CbL34fFNfCj0+F1quIK6lmI01NDQUMhslQY7LDkZKbpR6aAT/SuPOnNAnN6QRSBixz2h294BZWddgttJmFL0aNcgxKczHTUK95Fl7TrmniVHy+7SSK4dkrgvKMq+vgcTL3S1wML6Vn2qJOqRkKNvjPlnljyuo0y9eSonWV9B7VmodBWyQRRn152fXCVzpyO5PThIKO7dNWtmCsz+LazczW+9VjlvDNwXpRwTM0yZnswMz1p/f0/QaTLh5w599+jwj0TBQKvcKbh++/KLmGXjgrJpPEmMTmeTvCurB4zaP4z1dQ1+L7eXlk0IkmlwMVrRNrYm/U2PQycWpzThiKbVQ+8shHpykm7w7B+BUsswhlnE53NQBiiawFQOO/FwmhXzUSIj2CbTl06ksIbQyAAl9gmy9+HxTXwo9PhdariCupZiNNTQ0FDIbJUGOyw5GSm6UemgE/0rjzpzQJzekEUgYsc9odveAWVnXYLbSZhS9GjXIMSnMx01CveRZe065p4lR8vu0kiuHZK4LyjKvr4HEy90tcDC+lZ9qiTqkZCjb4z5Z5Y8rqNMvXkqJ1lfQe1ZqHQVskEUZ9edn1wlc6cjuT04SCju3TVrZgrM/i2s3M1vvVY5bwzcF6UcEzNMmZ7MDM9af39P0Gky4ecOfffo8I9EwUCr3Cm4fvvyi5hl44KyaTxJjE5nk7wrqweM2j+M9XUNfi+3l5ZNCJJpcDFa0Ta2Jv1Nj0MnFqc04Yim1UPvLIR6cpJu8OwfgVLLMIZZxOdzUAYomsBUDjvxcJoV81EiI9gm05dOpLCG0MgAJfYJsvfh8U18KPT4XWq4grqWYjTU0NBQyGyVBjssORkpulHpoBP9K486c0Cc3pBFIGLHPaHb3gFlZ12C20mYUvRo1yDEpzMdNQr3kWXtOuaeJUfL7tJIrh2SuC8oyr6+BxMvdLXAwvpWfaok6pGQo2+M+WeWPK6jTL15KidZX0HtWah0FbJBFGfXnZ9cJXOnI7k9OEgo7t01a2YKzP4trNzNb71WOW8M3BelHBMzTJmezAzPWn9/T9BpMuHnDn336PCPRMFAq9wpuH778ouYZeOCsmk8SYxOZ5O8K6sHjNo/jPV1DX4vt5eWTQiSaXAxWtE2tib9TY9DJxanNOGIptVD7yyEenKSbvDsH4FSyzCGWcTnc1AGKJrAVA478XCaFfNRIiPYJtOXTqSzkQU2IHXpMBvbRTYogwsIr
server:
  host: nextassets_platform
  port: 11451
  key: q9urXQ9SuUeS6CkztnqaDEn2qgXROqNWJ5lEfkoLj6U=

mysql:
  host: icecliffs
  port: 3306
  username: nextassets
  password: nextassets
  database: nextassets
```

3. 下载 nextassets 分析平台 `nextassets_platform-linux_x86_64.zip`
4. 接着配置 nextassets 分析平台里的 `config.yaml`

```yaml
mysql:
  host: icecliffs
  port: 3306
  username: nextassets
  password: nextassets
  database: nextassets

redis:
  host: icecliffs
  port: 6379
  password: "nextassets"
```

5. 启动扫描器对资产进行扫描

```
./nextassets_scanner -t 100 -h 1.0.0.0/8,2.0.0.0/8
```

6. 在分析平台查看对应的扫描结果

## 技术实现

目前主要分为三个端，分别是探针（扫描器，初步完成）、分析平台（态势整理，已完成）和研判平台（正在实现）

![image-20250709001409876](./assets/image-20250709001409876.png)

probe 端只需要负责扫描即可，而 trend 和 hunter 端需要考虑的东西可多了，probe 只负责采集资产信息然后推送到分析平台，而分析平台会对探针数据，做资产归类等分析，并且将结果可视化识别

扫描器独立运行，可分布在不同的机子上面，只需要保证数据库和平台消息队列配置没有出错即可


```bash
./scanner -t 100 -h 1.0.0.0/8,2.0.0.0/8
```

![image-20250709000130265](./assets/image-20250709000130265.png)

## 语法说明

首页有搜索框，可以输入相对应的语法进行资产搜索

> 搜索

![image-20250709231014612](./assets/image-20250709231014612.png)

- 具体语法

```bash
ip	查询资产IP	ip="116.62.191.10"
ip.port	端口 (context.ports)	支持 IN、=、!=
domain	域名	domain="iloli.moe"
hash	资产hash	hash="c4ca4238a0b923820dcc509a6f75849b"
country	国家	country="cn"
province	省份	province="福建"
create_time	创建时间	create_time>="1989-01-01"
update_time	更新时间	update_time>="1989-01-01"
```

![image-20250709231027226](./assets/image-20250709231027226.png)

## 数据展示

> 态势大屏

![image-20250709231057683](./assets/image-20250709231057683.png)

> 数据展示

![image-20250709231209776](./assets/image-20250709231209776.png)

> 敏感信息识别

![image-20250709231227911](./assets/image-20250709231227911.png)

> 站点截图

![image-20250709231252221](./assets/image-20250709231252221.png)

> 资产归纳整理

![image-20250709231715073](./assets/image-20250709231715073.png)

> 风险路径识别

![image-20250709231912082](./assets/image-20250709231912082.png)

![image-20250709231929425](./assets/image-20250709231929425.png)

> 资产证书关联 

![image-20250709232018463](./assets/image-20250709232018463.png)

图不传了，打码好累的说

## 论文参考

> 感谢各位专家提供的论文，本人在阅读完后发现什么都不懂，但还是硬着头皮写了出来，所以才会有很多bug

- CHEN Qing, LI Han, DU Yuejin, ZHANG Yirong, . Practice and thinking of cyberspace surveying and mapping technology[J]. Information and Communications Technology and Policy, 2021, 47(8): 30-38.
- 周杨,徐青,罗向阳,刘粉林,张龙,胡校飞.网络空间测绘的概念及其技术体系的研究[J].计算机科学,2018,45(5):1-7.
- 王宸东,郭渊博,甄帅辉,杨威超.网络资产探测技术研究[J].计算机科学,2018,45(12):24-31.
- 绿盟科技《网络空间地图测绘理论体系白皮书》https://www.shujiaowang.cn/uploads/20230702/db8df40f3c9a663ddd9ff1e203489940.pdf
- [ 全球网络空间测绘地图研究综述](https://jcs.iie.ac.cn/ch/reader/view_reference.aspx?pcid=5B3AB970F71A803DEACDC0559115BFCF0A068CD97DD29835&cid=8240383F08CE46C8B05036380D75B607&jid=09F0A586465924BAA255CE91FDD7C7DF&aid=E21E6737789C2A772C2343BD4521F616&yid=B6351343F4791CA3&vid=2B25C5E62F83A049&iid=94C357A881DFC066&sid=CA4FD0336C81A37A&eid=B31275AF3241DB2D)[J].微型机与应用
