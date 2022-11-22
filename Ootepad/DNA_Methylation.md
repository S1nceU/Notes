有線、無線
wireless 
no wire 
broadcast 散播 廣播

能量消耗快
距離越遠，收到的能量越弱
平方反比

reflection obstruction interference

無線的技術較困

microwave 
同步衛星 35000km

network core
"yeeeeee個爹~塔"
"yeeeeee大堆爹~塔"
packet-switching 封包交換
每一個packet走的路徑不一定相同

傳輸速率 link transmission rate(R) = 頻寬
L : 一個封包的長度

送出封包的時間
packet transmission delay 
= time needed to transmission L-bit packet into link 
= L(bits)/R(bits/sec)

store-and-forward 從一個設備到交換區，先存起來，然後再做轉送
有幾個交換區 就會有幾個 L/R

頻寬(R)越大 速率越快

bps = bits per second

ex.L = 10000bit R = 1Mbps頻寬   0.1ms 秒把一個封包送出 <- one-hop transmission delay 

queneing delay ,loss
loss : 在switch、RT滿的時候，封包就會被loss掉

forwarding 事後  routing 事前

circuit switiching 傳統的電話網路
會有一個專有頻寬  
dedicated resources : no sharing ,between source ans destination
需花時間建立線路連線
不用store and forward，所以就不會有queneing delay
容易造成佔線而浪費資源


throughput 傳輸路徑上 R最小的

Security 


internet protocol stack

application 應用  IMAP、SMTP、HTTP

transport   傳輸  終端到終端 TCP、UDP

network     網路  規劃路徑  IP、routing protocols  

link        連結  點對點的問題  Ethernet、802.11(WiFi)、PPP

physical    實體  bit在線上的傳輸


ISO 7層架構

封裝 (加標籤) Encapsulation 往下就是封裝    送出東西
解封裝        Decapsulation 往上就是解封裝  收到東西

chapter2
網路

socket
傳輸到port number 
32-bit address
HTTP port 80

TCP
reliable transport  
flow control        "控制data"不要太多
congestion control  "網路已經塞車"了，告訴source端不要再送了
does not provide    不能保證頻寬
connection-oriented TCP可以提供port number

UDP 就是快 不會管資料就直接傳
unreliable data transfer

大多網頁都用TCP

transport Layer Security (TLS)
provides encrtpted TCP connection
data integrity
end-point authentication 

URL

HTTP
HTTP usesnTCP
port 80

HTTP 1.1 
Non-persistent HTTP
response time : RTT的delay時間 1. initiate TCP connection 2. request file -> 兩個RTT時間
		    會關掉連線
persistent HTTP

HTTP request message 
ASCII (human-readable format)

HTTP GET / response 
stateless 

Web caches (proxy servers 代理伺服器)
可以將origin server的response記住
why use caching
reduce response time for client request
reduce traffic on an institution access link
internet is dense with caches

SMTP:push
HTTP,IMAP : pull

DNS:記名字比記數字好記，為了要解決所以用DNS
1.主機別名
2.郵件伺服器別名
3.*負載分配*  :複製的 Web 服務器：多個 IP 地址對應一個名稱
DNS name resolution :
1.iterated query :對於server的load較小  現今大多都用這個
2.recuersive query :以上相反
DNS用的是TCP的服務
DNS多用的是UDP的服務(正常查詢)

p2p:peer不是固定的server

CDNs
多跟視訊串流有關
CBR 產生一個基本畫面要多少頻，速率固定，多用低解析
VBR 速度會隨環境變化
MPEG4先多用於網路串流影片

DASH (Dynamic,Adaptive Streaming over HTTP)
在server的時候，影片切成很多個chunk
提供不同塊的URL
在client的時候，一次要一個chunk
決定使用頻寬的多寡 (決定權)
很多決定都是client端所做決定

CDNs
把多個影片分在很多個server 不會將所有影片都放在同一個server
離最近的server發出請求

OTT : Over The Top
用網路網路來達成檔案分配的目的

Socket programming
應用層的process用socket去連到其他服務
UDP unreliable datagram
TCP reliable, byte stream-orient
TCP -> server process must be runing
	 server收到請求的時候，server會創建一個新的socket與client建立獨立連線，做流量控制、傳輸錯誤訊息

CH2 應用層 Summary
利用現成下層的服務，建立新的經營模式。

Transport layer: 強調終端對終端的服務，邏輯的連線
透過port number來做連線
service not available:
	delay guarantees
	bandwidth guarantees

UDP: User Datagram Protocol
只提供port number
use on :
	streaming multimedia apps 
	DNS
	SNMP
	HTTP/3
32bit
best effort service 網路的服務只提供最基本的傳輸，沒有保證

Principles of reliable data transfer

rdt1.0 state machion
要傳送data時，會在data前面加一個標頭
有data來時，會看標頭的port number知道要送到哪一層

rdt2.0
ACK  取得的訊息沒有錯 (常用到)
NAK  有送錯的訊息
"Stop and wait" 對方確定收到了，才送下一個檔案！！ (回饋、timer、多編號)
wait for call from above <-> wait for ACK or NAK  (sender狀態的改變)

rdt2.1
那會不會傳回去送錯?
用一個bit來確認送出跟接收到的訊息
sent : 0資料送出後 若是接收到對了 就傳1資料
receiver : 0接收是對的 就等待1 或是錯了 就要等0 若是對了但得到重複的 就丟掉?收到重複會再回一次ACK

rdt2.2
只送ACK 因為可以直接判別對錯 達到相同的功能 
sender : 要等到接收到該街道的序號 ACK0 就該收到ACK0 若是收到ACK1就要重傳
receiver : 在等ACK0封包，卻收到ACK1，就代表錯了

#若傳輸過程封包不見了怎麼半？！  rdt1、rdt2無法解決
rdt3.0
sender->計時器等太久重送
從送出的那刻開始計時

為甚麼DNS是UDP

Go-Back N
sender:
成功收到就 sliding-window
如果沒收到前面的ACK，time out 就會重傳
timer針對最舊還沒收到ACK的，如果太久沒收到的話，就會從最舊的點往下都重送
如果收到一個ACK就會reset timer
receiver:
只會收到按照順序的data，如果沒有按照順序就會丟掉
但可以產生重複的ACK，去確認前面的都收到了

Selective repeat
雖然前面沒收到，收到了後面的，那就把後面的先暫存起來，等待收到前面的對的
sender:
每個data都有timer，time out 的話就只會重送超時的packet
receiver:
收到就先存起來
就算收到已經傳送過的ACK，還是得再送一次ACK
#如果資料序號(seq#)重複的頻率跟window size相當的話，就會出錯，所以盡量都會把重複的頻率做到window size的2倍以上

TCP
byte steam: 用packet的第一個byte做seq#
pipeline的方式
A > seq=42, ACK=79, data='C' > B
A < seq=79, ACK=43, data='C' < B
A > seq=43, ACK=80,          > B
How to estimate RTT?
　用分段傳輸的時間測量，如果中間有重送就不算進去，
　取一端時間做平均，且中間需smoother

整個標頭就有20byte，有
src port,dest port
seq#
ACK
length of TCP header,reveive window(flow control)
checksum,Urg data pointer
如果ACK要有意義的話 就必須要將 lenth of TCP 的A啟用

用過去的經驗估計time out時間以外，還要跟著RTT的變化量去，以防突然變化量突然提高或下降
SampleRTT:用最新的DATA的RTT來看現在網路的變化量
	    用平均估計的RTT跟最新的RTT，去作比重函式

以下公式大概知道就好
EstimatedRTT = (1 - a)*EstimatedRTT + a*SampleRTT
TimeoutInterval = RstimatedRTT + 4*DevRTT(safety margin)  加入緩衝空間

fast retransmit: 
當我重複收到 3 個重複的 ACK 的話，代表有傳輸的資料因為一些原因不見了，且可以判斷不是因為網路塞車，因為已經傳回來 3 個了

TCP flow control
由 receive window 來做 flow control 
//為了讓transmitter

RWND (Receiver WiNDow) 
隨著receiver的buffer來調整傳輸的速度 讓receiver不要overflow

TCP connection management
why don't use 2-way handshake?
* variable dalays
* retransmitted messages (e.g. req_conn(x)) due to message loss
* message reordering
* can't "see" other side
half open

Closing a TCP connection
雙方都可以開啟連線和關掉連線
關掉時，將 FINbit = 1 從client端送出
等著 server 端，回傳 FINbit = 1 的資訊
client 收到後可能會等待30秒左右，因為有可能還有其他資料尚未送到
				└--> timed wait for 2*max segment lifetime
Principles of congestion control ->  Causes/costs of congestion
congestion control 對於傳輸過程的情況做控制
throughput 真正送到目的端的速度

實際上時
application-layer input = application-layer output : λin = λout
transport-layer input includes retransmissions : λ'in >= λin   傳輸層看到的loading一定比應用層看到的還要大
所以要看傳輸的router的buffer是否有空間
輸入將近滿載時，會開始出現 time out ，所以會開始慢慢做congestion control，降低throughput

"cost" of congestion :
* more work (retransmission) for given receiver throughput
* unneeded retransmissions: link carries multiple copies of a packet

End-end congestion control
發現網路塞車，就開始放慢傳輸量
用預測的方式，如果太就收到ACK回復，就可能覺得網路塞車，所以開始放慢傳輸量，是自己預測，沒有資訊

Network-assisted congestion control 較少使用
會有一個router提供回饋，送資訊給end，告訴使用端塞車惹










































































