## Azure
+ 2010/2 正式發行
+ 全球有 54 座資料中心
+ 44 個 CDN
+ 有 30 餘種服務
+ 提供了 PaaS 雲端平台服務, Iaas 虛擬機器和網路服務, SQL 數據資料庫服務等

## IoT Hub
![](https://th.bing.com/th/id/R.67948e97c11e69032212ae49bec2b005?rik=xbV5GJVvtLtaNg&pid=ImgRaw&r=0 =50%x)

### Device
+ 創建設備的方式有許多種類
    + 直接在 IoT Hub 上直接新增
    + 用 API, SDK, DPS 等方式新增設備至 IoT Hub
+ 身分驗證
    + 對稱式金鑰
    + X.509
        + 自我簽署
        + 合法簽署
+ 連結 Hub 的方式
    + 連接字串 ( connection string )
    + DPS ( Device Provisioning Service )

### 簡介
+ IoT Hub 是一個託管於雲端的受控服務，作為 IoT 終端裝置與觀禮裝置之間的雙向通訊橋梁
+ IoT Hub能在IoT裝置與雲端控制後端間建立可靠且安全的通訊
+ IoT Hub支援監視可追蹤裝置的建立、裝置失敗，以及裝置連線等事件，以協助維護IoT解決方案的健全狀況。

### 架構
![](https://i.imgur.com/u8nxKDK.png)

### Reference
[IoT Hub 簡單介紹](https://hackmd.io/@KurtKoNote/S1pDyou1r#:~:text=%E9%80%B2%E5%85%A5Azure%E5%85%A5%E5%8F%A3%E7%B6%B2%E7%AB%99%EF%BC%8C%E5%88%B0%20IoT%20Hub%E8%A4%87%E8%A3%BD%E8%A6%81%E9%80%A3%E6%8E%A5%E8%A3%9D%E7%BD%AE%E7%9A%84%E5%90%8D%E7%A8%B1%EF%BC%9A%202.%20%E9%80%B2%E5%85%A5IoT%20Hub%EF%BC%8C%E9%BB%9E%E9%81%B8%E5%85%B1%E7%94%A8%E5%AD%98%E5%8F%96%E5%8E%9F%E5%89%87%EF%BC%8C%E9%BB%9E%E9%81%B8%22service%22%EF%BC%8C%E8%A4%87%E8%A3%BD%E9%80%A3%E6%8E%A5%E5%AD%97%E4%B8%B2%EF%BC%9A,3.%20%E9%80%B2%E5%85%A5%22azure-iot-samples-python-masteriot-hubQuickstartsback-end-application%22%20Azure%E7%AF%84%E4%BE%8B%E7%A8%8B%E5%BC%8F%E8%B3%87%E6%96%99%E5%A4%BE%EF%BC%8C%E9%96%8B%E5%95%9F%E7%AF%84%E4%BE%8B%E7%A8%8B%E5%BC%8F%EF%BC%9A%EF%BC%9A%204.%20%E5%A1%AB%E5%85%A5IoT%20Hub%E6%9C%8D%E5%8B%99%E9%80%A3%E6%8E%A5%E5%AD%97%E4%B8%B2%E8%B7%9F%E8%A3%9D%E7%BD%AE%E5%90%8D%E7%A8%B1%EF%BC%9A)

## Device Provisioning Service ( DPS )
+ 用於設定 IoT hub 的全自動 DPS 作業，透過 DPS，可以透過安全且可調整的方式 Provisioning 裝置

+ DPS 可分為兩個部分
    + 註冊裝置
        + 建立裝置以及 IoT solution 的連線
    + 設定
        + 根據解決方案的需求套用設定

+ 畫重點
    1. 是 IoT Hub 的一個配套服務
    2. 不用在 IoT Hub 進行配置就可以註冊 IoT 設備
    3. 安全、可縮放、可預配數百萬台


## Azure IoT DPS
+ 可以對 IoT Hub 進行自動 Just-in-Time 布建，完全無需人為手動介入。而中間的有憑證安全，且 DPS 可以快速擴充佈建數個裝置。

### 架構

![](https://learn.microsoft.com/zh-tw/azure/iot-dps/media/about-iot-dps/dps-provisioning-flow.png)

1. 裝置首次開啟，會先連線至 DPS 端點，並驗證憑證
2. 設備將識別性資訊傳遞給DPS
3. DPS 通過 X.509, TPM, 對稱式加密等方法驗證，從而驗證設備的標誌
4. DPS 將設備註冊到 IoT Hub，並增加設備所需的 Device twin
5. IoT Hub 將設備連線資訊會回傳給 DPS
6. DPS 將連線資訊回傳給設備，從而讓設備可以將數據傳到 IoT Hub
7. 設備可直接連線至 IoT Hub
8. 設備從其在 IoT Hub 獲得 Device twin 的資訊

### Enrollment type
+ 註冊群組
    + 可以透過 X.509 或是對稱式加密方法進行註冊。對於需要大量設備共享初始設定和配置非常友善
+ 個別註冊
    + 可用 X.509 裝置憑證或是 SAS 憑證作為憑證機制。適用於只需要唯一初始配置的設備或是只能透過 TPM 進行註冊等裝置

+ 管理註冊
    + 可以透過註冊群組控制是否要讓該中繼憑證上的裝置直接連上 IoT Hub
![[Pasted image 20250610180011.png]]


## X.509 Certification

+ X.509 是公鑰基礎設施 ( PKI ) 的標準格式，結構優勢在於公私鑰組成的密鑰對而構建的。
+ 基於 X.509 的 PKI 最常見的用例是使用 SSL 證書，讓網站與用戶之間實施 HTTPS 安全瀏覽
+ 優點
    + 建立信任
        + X.509 憑證密鑰結構允許驗證
        + 當憑證由受信任的 CA 簽章時，憑證用戶可以確信憑證所有人以及網域已經過驗證
        + 而若是自簽章憑證可信度較低，因為域名所有人無須任何驗證
    + 可擴展性
        + PKI 架構具有可擴展性，通過廣泛分發公鑰，保護機構網內外的訊息傳遞
        + 原因：攻擊者無法獲得解密所需的私鑰

![](https://learn.microsoft.com/zh-tw/azure/iot-dps/media/tutorial-custom-hsm-enrollment-group-x509/example-device-cert-chain.png)

+ 根憑證 root
您將完成 擁有權證明 ，以確認根憑證。 這項驗證可讓 DPS 信任該憑證，並驗證其所簽署的憑證。

+ 中繼憑證 intermediate
通常會使用中繼憑證，依生產線、公司部門或其他準則以邏輯方式將裝置分組。 本教學課程使用具有一個中繼憑證的憑證鏈結，但在生產案例中，您可能會有數個憑證。 此鏈結中的中繼憑證是由根憑證簽署。 此憑證會提供給 DPS 中建立的註冊群組，以邏輯方式將一組裝置分組。 此組態可讓您管理具有相同中繼憑證所簽署之裝置憑證的整個裝置群組。

+ 裝置憑證 device
裝置憑證 (有時稱為分葉憑證，) 會由中繼憑證簽署，並連同其私密金鑰一起儲存在裝置上。 在理想情況下，這些敏感性專案會以 HSM 安全地儲存。 每次嘗試布建時，每個裝置都會顯示其憑證和私密金鑰以及憑證鏈結。
[建置x.509自簽章憑證](https://learn.microsoft.com/zh-tw/azure/iot-dps/tutorial-custom-hsm-enrollment-group-x509?tabs=windows&pivots=programming-language-python )

+ 自我簽署的憑證檔，符合 X.509 標準
+ 在註冊 X.509 CA 憑證且裝置已簽署至憑證信任鏈結之後，最後一個步驟是在裝置連線時進行裝置驗證。 當以 X.509 CA 簽署的裝置連線時，它會上傳其憑證鏈結來進行驗證。 此鏈結包含所有中繼 CA 和裝置憑證。 有了此資訊，「IoT 中樞」就會以一個有兩步驟的程序來驗證裝置。 「IoT 中樞」會以密碼編譯方式驗證憑證鏈結來了解內部是否一致，然後向裝置發出所有權證明查問。 「IoT 中樞」會在從裝置獲得成功的所有權證明回應時，宣告裝置驗證。 此宣告會假設裝置的私密金鑰已受保護，而只有裝置能夠成功回應這個查問。 建議您在裝置中使用安全晶片 (例如「硬體安全模組」(HSM)) 來保護私密金鑰。
## Message routing
+ MR 是 IoT Hub 內置的一種訊息分發機制
+ MR 提供 DPS 訊息、Device life cycle、連線狀態
+ MR 可以有很多個端點和路由，方便後須可以檢視專案上的邏輯該念以及產品數據
+ 儲存格式
    + AVRO
    + JSON
+ 預設儲存格式
    + {iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}
    + HH 為 GMT 時間，需要 -8
    + partition 為設備為在 IoT Hub 的第幾個


## Digital Twin
+ 是 Azure IoT Hub 維護的一個 Json 檔案，每一個存在 IoT Hub 中的設備都有一個 Json 文件，同時提供可以查修的 API/SDK

![](https://learn.microsoft.com/zh-tw/azure/iot-hub/media/iot-hub-devguide-device-twins/twin.png)

+ Json 結構
    + Tag
        + 數據的基本資訊，像是安裝地址、使用者資訊...
        + 這些數據只能由 Server 端讀寫
    + Properties
        + Desire
            + Server 讀/寫，Client 讀/訂閱變更事件
            + Server 端進行修改，Client 端收到修改並進行操作
        + Reported
            + Server 讀，Client 讀寫
            + Client 上傳數值改變，Server 通過查詢，得知 Client 值是多少

>Azure Digital Twins允許我們通過空間智能圖來模擬人、地點和設備之間的關係。它允許我們對傳入的設備數據運行自定義函數，以根據接收到的數據提供自動操作，同時具有上下文和空間感知能力。它簡化了複雜模型的開發過程，並為傳統物聯網解決方案提供了新的、更成熟的功能。它利用雲的力量來擴展和交付多租戶解決方案，並允許垂直集成來整合其他技術解決方案，如混合現實和水平集成，以將多個物聯網解決方案連接在一起。

+ 簡單來說，透過修改以及讀取 Properties 改變和保持狀態，當今天 Server 要對 Client 做操作時，就可以改變 Desire 資料，使得 Client 得知修改訊息並執行相關操作

+ 而為甚麼要叫做 twin，因為 Client 就像是在 Server 端有了一個分身，當今天這個分身改變了甚麼，Client 的本體也要改變


