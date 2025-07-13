# NextAssets (全球隼)

> 下一代資產規劃，致力於打造全網最爛的、BUG最多、功能最少、並且資產最少、掃描與攻擊面基本沒有的攻擊面測繪平臺

---

<div align="center">  
    <img src="./assets/不安です.png"> 
</div> 

- [簡體中文](README.md)
- [繁體中文](README-zh-TW.md)
- [English](README-en-US.md)
- [조선어](README-ko-KP.md)

## 簡介

**NextAssets** 是一款面向賽博空間保安從業者與互聯網情報搜集保安的自動化資產發現與整理平臺，支持多維度的網絡空間測繪采集與多維度資產管理，覆蓋域名/IP/端口等基礎信息，支持HTTP/S、安全掃描、漏洞掃描、組件識別、指紋識別、自動化FUZZ、JS安全分析、SSL證書分析等功能，便於高效網絡空間測繪與威脅關聯分析，本系統會自動化對掃描到的數據進行數據清洗和去重，並且可以針對 SRC 項目進行自動化監控。

> PS：如果您覺得該項目眼熟，那就對了！如有巧合，純屬雷同。

在此之前，曾使用Python寫過一個非常垃圾的版本：http://github.com/icecliffs/Cliffscan

<div align="center">
<p>本項目所使用的技術僅供安全人員使用，出了事情後果自負，本項目僅供學習交流</p>
</div>


## 功能

- [x] 網站結構解析與 JS 路徑提取、多維度 XSS 掃描（無頭實時編譯網站並捕獲 XSS 端點信息）
- [x] 數據可視化支持（可拓展）、端口 / 協議 / 中間件識別，使用 DeepSeek 分析請求數據，自動化提取敏感信息和風險信息
- [x] 資產多維度識別（域名 / IP / 組件 / 指紋）、多維資產自動關聯（如：關聯 `bilibili.com` 相關 IP）
- [x] SSL 證書關聯與證書信息提取、網站截圖與 HTTP 響應分類、語法搜索功能、分布式掃描、HOST碰撞等技術發現隱藏資產
- [x] IPv6 支持、掃描器認證機製、蜜罐 / 蜜網識別、周期掃描、SRC 定製掃描、請求/響應分析
- [ ] 全球資產測繪進度（娛樂用）<img src="https://img.shields.io/badge/%E8%BF%9B%E5%BA%A6-0.80%25(30000000/3706585103)-brightgreen)"/>，自2025年7月9日以來

## 安裝

1. 數據庫服務最好跑在 Docker 裏，如果宿主出了問題我擔負不起，只需要跑 Redis 和 MySQL 即可
2. 然後運行掃描器  `nextassets_scanner-v0.0.1-macos-arm64`
3. ~~接著配置 nextassets 掃描器裏的 `config.yaml`，需要到 [Next Assets 授權中心](https://nextassets.iloli.moe) 進行一機一碼認證授權，將授權碼貼到配置文件裏~~，不需要授權了，我懶得寫，愛咋掃咋掃吧，只需要配置mysql就行

4. 下載 nextassets 分析平臺 `nextassets_platform-v0.0.1-macos-arm64`
5. 接著配置 nextassets 分析平臺裏的 `config.yaml`

```yaml
server:
  host: 127.0.0.1
  port: 8080

mysql:
  host: 127.0.0.1
  port: 3306
  username: nextassets
  password: 123456
  database: nextassets

redis:
  host: 127.0.0.1
  port: 6379
  password: "123456"
```

5. 啟動掃描器對資產進行掃描，掃描器最好下載 GeoLite2-City.mmdb 和 GeoLite2-ASN.mmdb 來獲取 ASN 和地理位置（如果掃全網的話），具體參數自己看吧，不想寫了

```
./nextassets_scanner-v0.0.1-macos-arm64 -t 64 -h 1.0.0.0/8,2.0.0.0/8 -p 198,2003
```

![image-20250713155509753](./assets/image-20250713155509753.png)

6. 在分析平臺查看對應的掃描結果（分析平臺因為作者自己掃的全網，所以緩存時間設置為12小時🧐，建議前期使用語法搜索你掃描的網段 `ip="192.168.50.0/24"`）**分析平臺匯會自動初始化數據庫，建議設置數據庫名為 nextassets**，然後查詢也有緩存，具體12分鐘刷新一次，所以還是建議等掃描完後在查詢。。。

```
./nextassets_platform-v0.0.1-macos-arm64
```

![image-20250713155517716](./assets/image-20250713155517716.png)

7. 最後到分析平臺看看你掃了什麽東西吧，enjoy :)

## 技術實現

目前主要分為三個端，分別是探針（掃描器，初步完成）、分析平臺（態勢整理，已完成）和研判平臺（已基本實現，能對所有資產進行安全分析，引入 fscan 進行安全風險檢測）

![image-20250709001409876](./assets/image-20250709001409876.png)

probe 端只需要負責掃描即可，而 trend 和 hunter 端需要考慮的東西可多了，probe 只負責采集資產信息然後推送到分析平臺，而分析平臺會對探針數據，做資產歸類等分析，並且將結果可視化識別

掃描器獨立運行，可分布在不同的機子上面，只需要保證數據庫和平臺消息隊列配置沒有出錯即可

我的掃描方：亞馬遜、Vultr 等 VPS 各有 3 臺機子，節點分別為日本、香港、美國、新加坡、臺灣，然後發起掃描，最後返回數據到某臺服務器上

## 語法說明

首頁有搜索框，可以輸入相對應的語法進行資產搜索

> 搜索

![image-20250713162049187](./assets/image-20250713162049187.png)

- 具體語法

```bash
ip	查詢資產IP	ip="116.62.191.10"
ip.port	端口 (context.ports)	支持 IN、=、!=
domain	域名	domain="iloli.moe"
hash	資產hash	hash="c4ca4238a0b923820dcc509a6f75849b"
country	國家	country="cn"
province	省份	province="福建"
create_time	創建時間	create_time>="1989-01-01"
update_time	更新時間	update_time>="1989-01-01"
```

## 數據展示

> 態勢大屏

![image-20250713164642611](./assets/image-20250713164642611.png)

> 數據展示

![image-20250713160959980](./assets/image-20250713160959980.png)

> 敏感信息識別

![image-20250713164757443](./assets/image-20250713164757443.png)

> 站點截圖

![image-20250713164829394](./assets/image-20250713164829394.png)

> 資產歸納整理

![image-20250713164940913](./assets/image-20250713164940913.png)

> 風險路徑識別

![image-20250713164838437](./assets/image-20250713164838437.png)

圖不傳了，打碼好累的說

## 論文參考

> 感謝各位專家提供的論文，本人在閱讀完後發現什麽都不懂，但還是硬著頭皮寫了出來，所以才會有很多bug

- CHEN Qing, LI Han, DU Yuejin, ZHANG Yirong, . Practice and thinking of cyberspace surveying and mapping technology[J]. Information and Communications Technology and Policy, 2021, 47(8): 30-38.
- 周楊,徐青,羅向陽,劉粉林,張龍,胡校飛.網絡空間測繪的概念及其技術體系的研究[J].計算機科學,2018,45(5):1-7.
- 王宸東,郭淵博,甄帥輝,楊威超.網絡資產探測技術研究[J].計算機科學,2018,45(12):24-31.
- 綠盟科技《網絡空間地圖測繪理論體系白皮書》https://www.shujiaowang.cn/uploads/20230702/db8df40f3c9a663ddd9ff1e203489940.pdf
- [ 全球網絡空間測繪地圖研究綜述](https://jcs.iie.ac.cn/ch/reader/view_reference.aspx?pcid=5B3AB970F71A803DEACDC0559115BFCF0A068CD97DD29835&cid=8240383F08CE46C8B05036380D75B607&jid=09F0A586465924BAA255CE91FDD7C7DF&aid=E21E6737789C2A772C2343BD4521F616&yid=B6351343F4791CA3&vid=2B25C5E62F83A049&iid=94C357A881DFC066&sid=CA4FD0336C81A37A&eid=B31275AF3241DB2D)[J].微型機與應用
