[計算機網路概論 - HackMD](https://hackmd.io/@frakw/BkeoRUtFP#13-network-core)
## 無線
wireless,no wire 
broadcast 散播 廣播
能量消耗快 距離越遠 收到的能量越弱
- reflection 
- obstruction 
- interference
無線的技術較困
microwave 
同步衛星 35000km
network core
"yeeeeee個爹~塔"
"yeeeeee大堆爹~塔"
## packet-switching 封包交換
每一個packet走的路徑不一定相同

$d_{total} = d_{proc} + d_{queue} + d_{trans} + d_{prop}$
nodal processing: 設備的處理時間

傳輸速率 link transmission rate(R) = 頻寬
L : 一個封包的長度

送出封包的時間
**packet transmission delay 
= time needed to transmission L-bit packet into link 
= L(bits)/R(bits/sec)

store-and-forward 從一個設備到交換區，先存起來，然後再做轉送
有幾個交換區 就會有幾個 L/R
頻寬(R)越大 速率越快

bps -> bits per second

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

## internet protocol stack
application 應用  IMAP、SMTP、HTTP

transport   傳輸  終端到終端 TCP、UDP

network     網路  規劃路徑  IP、routing protocols  

link        連結  點對點的問題  Ethernet、802.11(WiFi)、PPP

physical    實體  bit在線上的傳輸


ISO 7層架構

封裝 (加標籤) Encapsulation 往下就是封裝    送出東西
解封裝        Decapsulation 往上就是解封裝  收到東西

## Socket
傳輸到port number 
32-bit address
HTTP port 80

## TCP
reliable transport  
flow control        "控制data"不要太多
congestion control  "網路已經塞車"了，告訴source端不要再送了
does not provide    不能保證頻寬
connection-oriented TCP可以提供port number
大多網頁都用TCP

## UDP 
就是快 不會管資料就直接傳
unreliable data transfer

transport Layer Security (TLS)
provides encrtpted TCP connection
data integrity
end-point authentication 

URL
HTTP
HTTP usesnTCP
port 80

## HTTP 
1.0 -> Persistent HTTP
1.1 -> Non-Persistent HTTP
response time : RTT的delay時間 1. initiate TCP connection 2. request file -> 兩個RTT時間
會關掉連線


HTTP request message 
ASCII (human-readable format)

HTTP GET / response 
stateless 

## Web caches (proxy servers 代理伺服器)
可以將origin server的response記住
why use caching
reduce response time for client request
reduce traffic on an institution access link
internet is dense with caches

**SMTP:push,pull
HTTP,IMAP : 看信(一個是雲端另一個是載下來)

## DNS
記名字比記數字好記，為了要解決所以用DNS
1.主機別名
2.郵件伺服器別名
3.**負載分配**  :複製的 Web 服務器：多個 IP 地址對應一個名稱
###### DNS name resolution :
1.iterated query :對於server的load較小  現今大多都用這個
2.recuersive query :以上相反
DNS用的是TCP的服務

**P2P:peer不是固定的server

## CDNs
多跟視訊串流有關
CBR 產生一個基本畫面要多少頻，速率固定，多用低解析
VBR 速度會隨環境變化
MPEG4先多用於網路串流影片

## CDNs
把多個影片分在很多個server 不會將所有影片都放在同一個server
離最近的server發出請求
![[Pasted image 20221024092037.png]]

## DASH (Dynamic,Adaptive Streaming over HTTP)
在server的時候，影片切成很多個chunk
提供不同塊的URL
在client的時候，一次要一個chunk
決定使用頻寬的多寡 (決定權)
很多決定都是client端所做決定

## OTT : Over The Top
用網路網路來達成檔案分配的目的

## Socket programming
應用層的process用socket去連到其他服務
UDP unreliable datagram
TCP reliable, byte stream-orient
* server process must be runing
* server收到請求的時候，server會創建一個新的socket與client建立獨立連線，做流量控制、傳輸錯誤訊息

CH2 應用層 Summary
利用現成下層的服務，建立新的經營模式。

Transport layer: 強調終端對終端的服務，邏輯的連線
透過port number來做連線
service not available: 
*  delay guarantees
*  bandwidth guarantees

## UDP: User Datagram Protocol
只提供port number
use on :
* streaming multimedia apps 
* DNS
* SNMP
* HTTP/3
32bit
best effort service 網路的服務只提供最基本的傳輸，沒有保證

Principles of reliable data transfer

## rdt1.0 state machion
要傳送data時，會在data前面加一個標頭
有data來時，會看標頭的port number知道要送到哪一層

## rdt2.0
ACK  取得的訊息沒有錯 (常用到)
NAK  有送錯的訊息
"Stop and wait" 對方確定收到了，才送下一個檔案！！ (回饋、timer、多編號)
wait for call from above <-> wait for ACK or NAK  (sender狀態的改變)

## rdt2.1
那會不會傳回去送錯?
用一個bit來確認送出跟接收到的訊息
sent : 0資料送出後 若是接收到對了 就傳1資料
receiver : 0接收是對的 就等待1 或是錯了 就要等0 若是對了但得到重複的 就丟掉?收到重複會再回一次ACK
Sender：
![[Pasted image 20221116000219.png]]
Receiver：
![[Pasted image 20221116000250.png]]
## rdt2.2
只送ACK 因為可以直接判別對錯 達到相同的功能 
sender : 要等到接收到該街道的序號 ACK0 就該收到ACK0 若是收到ACK1就要重傳
receiver : 在等ACK0封包，卻收到ACK1，就代表錯了
![[Pasted image 20221116000347.png]]
#若傳輸過程封包不見了怎麼半？！  rdt1、rdt2無法解決

## rdt3.0
sender->計時器等太久重送
從送出的那刻開始計時
為甚麼DNS是UDP

## Go-Back N
sender:
成功收到就 sliding-window
如果沒收到前面的ACK，time out 就會重傳
timer針對最舊還沒收到ACK的，如果太久沒收到的話，就會從最舊的點往下都重送
如果收到一個ACK就會reset timer
receiver:
只會收到按照順序的data，如果沒有按照順序就會丟掉
但可以產生重複的ACK，去確認前面的都收到了

## Selective repeat
雖然前面沒收到，收到了後面的，那就把後面的先暫存起來，等待收到前面的對的
sender:
每個data都有timer，time out 的話就只會重送超時的packet
receiver:
收到就先存起來
就算收到已經傳送過的ACK，還是得再送一次ACK
// 如果資料序號(seq#)重複的頻率跟window size相當的話，就會出錯，所以盡量都會把重複的頻率做到window size的2倍以上

## TCP
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
#### SampleRTT:
用最新的DATA的RTT來看現在網路的變化量 用平均估計的RTT跟最新的RTT，去作比重函式

以下公式大概知道就好
EstimatedRTT = (1 - a)*EstimatedRTT + a*SampleRTT
TimeoutInterval = RstimatedRTT + 4*DevRTT(safety margin)  加入緩衝空間

#### fast retransmit: 
當我重複收到 3 個重複的 ACK 的話，代表有傳輸的資料因為一些原因不見了，且可以判斷不是因為網路塞車，因為已經傳回來 3 個了

## TCP flow control
由 receive window 來做 flow control 
//為了讓transmitter

#### RWND (Receiver WiNDow) 
隨著receiver的buffer來調整傳輸的速度 讓receiver不要overflow

TCP connection management
why don't use 2-way handshake?
* variable delays
* retransmitted messages (e.g. req_conn(x)) due to message loss
* message reordering
* can't "see" other side
half open

#### Closing a TCP connection
雙方都可以開啟連線和關掉連線
關掉時，將 FINbit = 1 從client端送出
等著 server 端，回傳 FINbit = 1 的資訊
client 收到後可能會等待30秒左右，因為有可能還有其他資料尚未送到
				└--> timed wait for 2 *max segment lifetime
Principles of congestion control ->  Causes/costs of congestion
congestion control 對於傳輸過程的情況做控制
throughput 真正送到目的端的速度

實際上時
application-layer input = application-layer output : λin = λout
transport-layer input includes retransmissions : λ'in >= λin   傳輸層看到的loading一定比應用層看到的還要大
所以要看傳輸的router的buffer是否有空間
輸入將近滿載時，會開始出現 time out ，所以會開始慢慢做congestion control，降低throughput

## "cost" of congestion :
* more work (retransmission) for given receiver throughput
* unneeded retransmissions: link carries multiple copies of a packet

#### End-end congestion control
發現網路塞車，就開始放慢傳輸量
用預測的方式，如果太就收到ACK回復，就可能覺得網路塞車，所以開始放慢傳輸量，是自己預測，沒有資訊

#### Network-assisted congestion control 較少使用
會有一個router提供回饋，送資訊給end，告訴使用端塞車惹

#### TCP congestion control: AIMD (Additive Increase Multiplicative Decrease)
線性增加 -> 發覺塞車後 -> 將傳送速度減到一半 -> 線性增加     (TCP Reno)  -> 像是鋸齒狀
如果發覺ACK回傳不了，發生loss， 將速度降成1RTT傳1個封包  (TCP Tahoe)

cwnd(Congestion WiNDow) 一個RTT可以傳多少封包
TCP rate ~= cwnd/RTT (bytes/sec)

## TCP slow start 
當connection開始建立的時候，
從cwnd = 1MSS 開始，當收到ACK後，每次兩倍的增加cwnd，
一開始很慢，但後來會成指數型成長

#### From slow start to congestion avoidance
當slow start開始塞車後，會設定一個門檻值，ssthresh，塞車的cwnd的1/2
之後會從ssthresh增加時，每次都只加一個，成線性成長

## TCP CUBOC (Briefly)
沒有塞車的時候，想把傳輸的東西跟速度變得更大
將傳輸的速度成三次函式的成長

#### TCP and the congested "bottleneck link"
塞車的瓶頸

#### Delay-based TCP congestion control 
用RTT的時間去判斷，要增加cwnd或是減少

## ECN (Explicit congestion notification)  
用ECN代表傳輸過程中有router開始塞車了  (網路層)
在ACK的欄位的ECE設為1，代表傳輸過程中有塞車  (傳輸層)  現實的確有這個欄位，但幾乎不會使用

TCP fairness
Is TCP fair ?
Yes, when under idealized assumption  在理想的情況下 (都像是鋸齒狀的控制速度)
* same RTT
* fixed number of sessions only in congestion avoidance

---
## Network-layer functions
network-layer function:
forwarding
routing
networl layer protocols in **every internet device** : hosts, routers
#### Data plane
local 
**forwarding**
傳統的 router，只看目的端，就直接傳送到目的地 
跟其他的 router 叫喚資訊，得知要從哪個 output port 出去

#### Per-router control plane 
將 data forwarding table 放在自己的 router
(現在大部分都是)
#### Software-Defined Networking (SDN) control plane 軟體定義網路
**routing**
<font color = "orange">中央控制 Remote Controller，集中化，指定 router 的 data forwarding table</font>
把所有 table 送到每一個 router
現在的傳輸速率已經相當快速，可以隨時做update
![[Pasted image 20221121092651.png]]

| Network Architecture | Service Model | Bandwidth | Loss | Order | Timing |
| --- | --- | --- | --- | --- | --- |
| Internet | best effort | none | no | no | no |
| ATM | 

## Router architecture overview
#### Input port functions
line termaination -> link layer protocol -> lookup, forwarding
看IP層目的端的欄位
physical layer
link layer
decebtealized switching: 
- match plus action
- line speed
- input port queuing 

#### Destination-based forwarding
forwarding table: 用目的地的位址範圍規畫路經
##### Longest prefix matching
用傳入目的端位址的前面數個數字確認要從哪個 Link interface 傳出
前面若有相同匹配的時候，要先看最長的是否匹配
ex. 
![[Pasted image 20221121095743.png]]
->
![[Pasted image 20221121095801.png]]
前面的數值與 1、2 所匹配，所以先看最長的 1 

#### Switching fabrics
分配路徑
三種：memory、bus、interconnection network
##### memory
當N越來越大時，memory 要超級快，否則會卡住
速度要2N
##### bus
速度是N倍
##### interconnection network
Crossbar 
速度跟 line speed 就可以了
大型交換機多用這種方法

#### Input port queuing
input buffer 要看 switch 夠不夠快才需要
##### Head-of-the-Line ( HOL ) blocking
因為 switching 不夠快， 導致原本時間該送到的 data ，卻被前面的 data 擋住
因為有 input port queuing 才會有的問題

#### Output port queuing
Scheduling discipline: 選擇哪個 PKT 送出
Buffer: 
Buffer 要多大 -> $RTT * C / \sqrt{N}$
##### Buffer Management
drop: 
- tail drop: dorp arriving PKT
- priority: 有權重，用優先級刪除
marking:
對資料標記，可以知道資料的重要性或是用處

#### Packet Scheduling:
+ 先進先出 ( FCFS )
+ 權重、優先級 ( priority )
+ round robin
+ weighted fair queueing

##### Schedling policies: priority
會先送出高優先權的資料再送其他的
且有先進先出的概念
後面還又分搶占跟非搶占等議題

##### Schedling policies: round robin
輪流傳輸

##### Schedling policies: weighted fair queneing
WFQ:
有比例權重的分配，但依然會輪流
$W_i / (\Sigma_j * W_j)$

#### Sidebar: Network Neutrality
網路中立

## IP: the Internet Protocol
#### IP Datagram format
長度: 32bits，原則上標頭有20bytes
實際上最多可用 1500 bytes or less
flgs: 若超過最大長度，就會將 IP 分割
##### time to live ( TTL ): 
每經過一個 router 就會減1，不想讓封包在網路上面亂跑，到0時，就會被丟掉。當 TTL 減1過後，在傳送過程的 checksum 就必須重新計算過一次，所以每經過一個 router 就會重算一次

#### IP addressing 
一個 *interface* 都會有一個網域( subnet )，一個介面卡就會有一個 IP address
都是1跟都是0會保留！！
subnet mask: /24 ( 不一定只用24，最常用就是24 ) 遮罩前24個 bits。用前面24個 bits 來確認是不是該網域
用 11111111 11111111 11111111 00000000 做 AND
( 255.255.255.0 )
每一個網域都有一個 subnet mask

##### CIDR ( Classless InterDomain Routing )
![[Pasted image 20221128100222.png]]
網域住址包含幾個 bits
a.b.c.d/x $\rightarrow$ x 就是網域是前幾個 bits

#### DHCP: Dynamic Host Configuration Protocol
UDP
+ "plug-and-play" 不需要人工設定
+ when join 時可以從 DHCP server 拿到一個 IP address，為臨時的
+ " 使用 *UDP* "
+ DHCP discover
	用 Broadcast 的方法，送出訊息，送出的訊息後面的 IP address，全部都會是"1"
+ DHCP offer
	一樣用廣播，傳出可用的 IP address，以及可使用的時間
+ DHCP request
	要求受到的 IP，說要使用
+ DHCP ACK
	確定以及使用得到的 IP address

同時會告訴你上網需要的資訊
first hop 的 router ( 第一個必須經過的 router )
DNS server 名稱跟 IP
網路的遮罩

##### 32-bits IP address 是否夠用?
否，在2011年已經用完
Sol: 
1. NAT的方式
2. IPv6有 128-bit address space

#### NAT (network address translation)
會有一個對外的 IP address，然後有一個 NAT translation table 對應外面跟裡面的 port 號
![[Pasted image 20221130111620.png]]
對內的 IP 多見的是 192.168/16
**在一個具有NAT功能的路由器下的主機並沒有建立真正的IP位址，並且不能參與一些網際網路協定**

#### IPv6
長度一樣為 32 bits (騙人的)
address 長度可以到 128-bit
跟 IPv4 差別: 都是為了節省時間，目的就是要快
+ no checksum
+ no fragmentation/reassembly 不用切割
+ no options

#### IPv6 and IPv4
若傳輸中間有 router 只看得懂 IPv4
所以要將 IPv6 加一個包裝，為 IPv4 的![[Pasted image 20221130114007.png]]
以上技術 $\rightarrow$ " **Tunneling** "

## Generalized Forwarding, SDN (Soft Define Network)
#### Generalized forwarding
+ destination-based forwarding
+ generalized forwarding

#### Flow table abstracton (forwarding table)
+ match
+ action
+ priority
+ counters

#### OpenFlow
match -> match 欄位
action -> 動作

#### abstraction
match+action: abstraction unifies different kinds of devices
##### Router
##### Switch
##### Firewall
##### NAT


## Middleboxes


#### Where's the intelligence?
20th century phone net: 電信網路
+ intelligence / computing at networks switches
Internet 現階段網路
+ intelligence, computing at edge
Internet
+ programmable network devices
+ intelligence, computing, massive application-level ingrastucture at edge

## Routing protocols
#### Graph abstraction : link costs
![[Pasted image 20221207101943.png]]
graph: $G = ( N, E )$
$N$: set of routers 所有 router 的集合 ex. { u, v, w, x, y, z }
$E$: set of links 所有點到點路徑的集合 ex.{ (u,v), (u,x), (v,x), (v,w), (x,w), (x,y), (w,y), (w,z), (y,z) }

#### Routing algorithm classification 路徑演算法
##### Global
必須知道每一個點跟路徑，知道網路的所有拓樸，才可以算出最短距離，所以需要 link state
##### Decantralized 分離
“distance vector” algorithms
##### Dynamic
##### Static

#### Link state
##### Dijkstra's link-state routing algorithm
###### centralized 集中
需要知道所有節點以及 cost 的訊息
所有節點都有相同信息 ?
每個點都必須知道所有點的存在
###### iterative 迭代
路徑中不管到哪個點，一定知道在那點前面的路徑一定最短的，代表到那個點的最短路徑已經知道了

跑完就知道初始點到所有點的最短路徑

$C_{x,y}$ 路徑長 若不是鄰居就會等於 無限大
$D( v )$ 目前到 v 的最短路徑長
$p( v )$ 目前為止最短路徑的前一個節點
$N'$ 最短路徑的節點集合

判斷公式 : $D( b ) = min ( D( b ), D( a ) + C_{a,b} )$
**like uniform-cost search**
For example:
![[Pasted image 20221207105450.png]]
然後就可以得知下方圖，做出一個 forwaring table
![[Pasted image 20221207110226.png]]

###### Oscillations possoble 可能震盪
當 link cost 取決於流量時，可能會產生震盪，路徑會不停變換

#### Distance vector
Base on *Bellman-Ford ( BF )* equation ( dynamic programming )
從初始端得知鄰居到目的地的最短路徑，再去判斷要選哪條
like A* search

iterative, asynchronous 迭代, 非同步
distributed, self-stopping

###### link cost changes
當 cost 降低的話 : 會很快就會全部更新
當 cost 升高的話 : 會慢慢收斂，要很久才會穩定，count-to-infinity problem

extra. 解決辦法 : poisoned reverse

## Irtra-ISP routing: OSPF
#### Inter approach to scalable routing
" **autonomous system** " a.k.a " **domain** " a.k.a " **AS** "
##### intra-AS ( intra-domain ) : 
routing among within same AS ( " network " )
gateway router: at " edge " of its own AS, has link to router in other AS'es
##### inter-AS ( inter-domain ) : 
routing **among** AS'es

#### Interconnected ASes
intra routing 決定網域中的 routing
inter routing 決定跨網域的 routing

#### Intra-AS routing: routing within as AS
most common intra-AS routing protocols
+ RIP: Routing Information Protocol [ RFC 1723 ]
	+ classic DV
+ EIGRP 看過就好
+ OSPF : Open Shortest Path First [ RFC 2328 ] 現今多用

##### OSPF
uses Dijistra's algorithm

## Routing among ISPs: BGP
#### BGP ( Border Gateway Protocol )
+ the de facto inter-domain routing protocol
	將網路可以結合在一起做傳輸的中間人，將網路做連結的協定
	" **glue that holds the Internet together** "
+ 使別人知道子網域的存在，在哪裡
+ BGP 分兩類
	+ **eBGP**: external，把鄰近網域可知道到達自己的網域的 reachablilty information
	+ **iBGP**: internal，向所有AS內部的 router 了解可達性訊息
	+ 根據 reachability information 跟 policy，決定好的 rout 路徑
![[Pasted image 20221212094925.png]]

#### BGP basic
+ BGP is a " path vector " protocol
![[Pasted image 20221212095756.png]]
告訴各 AS 如何傳遞到 X 的路徑

![[Pasted image 20221212100041.png]]

#### Hot potato routing
選擇本地 AS 離可以傳外往最近的 router，再將資料路徑往那邊傳

## SDN control plane
照表操課
下層傳輸的方式是用遠端的伺服器控制
更輕鬆的網路管理 : 避免路由器的配置錯誤，傳輸流量的靈活性更大

分三層 : 
	應用層 : routing、access control、load balance......
\--------------
北向API
	控制層 SDN Controller ( network OS )
南向API
\--------------
	實體層

#### Components of SDN controller

#### OpenFlow protocolC
+ **TCP** used to exchange messages
	+ optional encryption
+ Classes of OpenFlow messages
	+ controller-to-switch
	+ switch to controller

## Internet Control Message Protocol
未來的網路功能
NFV = Network Function Virtual 

#### ICMP
+ used by hosts and routers to communicate nerwork-level information
	+ error reporting: unreachable host, network, port, protocol
	+ echo request / reply ( used by ping )
+ network-layer "**above**" IP:
	+ ICMP messages carried in IP datagrams

+ **ICMP message** : type code plus first 8 bytes od IP datagram causing error

#### Traceroute and ICMP ICMP的追蹤路徑

## network management, configuration 網路管理、配置
#### Network Management
有一個 server ，可以知道網路的狀態或開啟、管理 device 的功能

#### Approaches to management
##### SNMP / MIB
+ operator queries / sets devices data ( MIB ) using Simple Network Management Protocols ( SNMP )

##### NETCONF / YANG
+ YANG: data modeling language、XML

#### SNMP protocol

## The Link Layer and LANs
兩個device真正傳輸線的問題
+ layer-2 packet: frame, encapsulates datagram
	+ 會在封包前後都加包裝

#### Where is the link layer implemented
+ 每個用戶端之間
+ 網卡或是晶片

## Error detection, correction $\star$
#### EDC: error detection and correction bits 錯誤偵測、更正
傳統的電腦網路都是 error detection
為了達到EDC，所以會在封包後面多加 bits 來做偵測
大部分在太空通訊的時候才會多用 error correction，

## Cyclic Redunancy Check ( CRC )
+ 更加強大的 error-detection coding
+ **D** : data bits 
+ **G** : bit patten ( generator ), of *r+1* bits ( given )
目標：選擇 r 個 CRC 位 R，使得 <D,R> 可以被 G 整除

![[Pasted image 20221219093308.png]]
+ $D*2^r$ = D + ( G bits數 - 1 )
+ R 是餘數
				**D      R**
sender 會送出 101110 011 G = 1001
recevier 會收到過後 用1001再長除法一次，若可以整除 ( 沒有餘數 )，那就是正確傳輸

## Multiple access protocols
在互相傳輸之間，不能讓每個人都知道有 data 在傳送，而有可能會撞在一起
甚至連控制訊息也有可能撞在一起
-> 讓控制頻段讓真正 data 的頻段分開

MAC ( Medium Access Control )
**given :** MAC of rate R bps
desiderata
+ one node, rate R
+ M nodes, rate R/M
+ 全部離散、分散

## MAC protocols: taxonomy
+ **Channel partitioning** ( fixed assignment )
	+ 切割時間、分頻率傳送
	+ 因為有規定好，所以不會有碰撞
+ **Random access**
	+ 隨機傳輸
	+ 等碰撞在處理
+ **" taking turns "**
	+ 輪流送

## Channel partitioning MAC protocols
#### TDMA: time division multiple access
slot : 時槽
規定時間一個 frame
不會有碰撞
每個人都得對時才可以，時間要同步
價錢cost太高!!

#### FDMA: frequency division multiple access
是用頻率來分割

## Slotted ALOHA
+ 會等每個 slot 的開頭才會重送封包
+ 發現碰撞時，就用機率的方式決定是否要重送
+ operation 
	+ 如果沒有衝突：節點可以在下一個時脈發送新 slot
	+ 如果有了衝突：節點會在每個後續時脈以 p 重傳 slot，直到成功
送資料時才會有同步動作
有可能會有衝突，也可能造成浪費

#### efficiency 效率
每個時曹都可以傳送一個，機率為 $p(1-p)^{N-1}$
每個重送的機率為 $Np(1-p)^{N-1}$
0.37
## Pure ALOHA
+ 不用做同步電路，但成功的機率會變低
+ 封包不會等到在 slot 的開頭傳送
+ 會增加碰撞機率
0.18
## CSMA ( carrier sense multiple access )
會檢視線路上是否有人在傳輸，listen before transmit
但還是無法避免碰撞
因為可能聽不到比較遠的傳輸資料

CSMA/CD 傳輸過後繼續聽 

## 乙太網路

#### 實體拓墣

在初期得時候乙太網路是使用匯流排的方式，但現今乙太網路是採用交換器連接，大幅減少了封包碰撞的機會。

#### 乙太網路frame架構

Frame在傳輸之前並不需要同步，Frame結構中有preamble的欄位，其數值固定為10101011，其用意在告知接收方接收的設定。

Frame中的位置是使用MAC位置來定位。

Frame中也有Type欄位，用於表示上層協定，現金大部分都是IP。
## 交換器
#### 交換器: 多重同步傳送

交換器的每個介面都有暫存器，當多個主機與同個主機溝通，比較晚到的那個封包會被存進暫存器中。

交換器的每個介面都是全雙工的，也就是說它不會有碰撞。

#### 交換器：自我學習

當封包到達交換器時，他會查看switch table是否有相關欄位，假如沒有來源資料則將主機位置與連接介面記錄起來，假如說沒有目標資料，則此封包會廣播出去。時間一久Switch Table中就會有所有介面的資訊。

## VLAN

#### Port based VLANs

使用交換器的實體介面來區分虛擬區網。

#### VLANs spanning mutiple switch

就是在交換器的某一個port設定成trunk port用於介接不同交換器，乙連接不同交換器上的虛擬網路。

## Datacenter networks
標準的資料中心有三層的routing
multipath 多層路徑：中間的路徑不會有重複的，所以會有很多多餘的路徑，才不會一條路斷了，就全斷了

load balancer ( 負載平衡 )：
+ 在**應用層**路由
+ 為了分配資源，他是一個代表群體的 server，收到過後再幫你轉接
+ 可對用戶隱藏內部資料結構

#### protocol innovations
+ link layer 
+ transport layer
+ routing, management: **SDN**

## 日常生活的網路請求
scenario 設想：requests to www.google.com
一開始不知道自己的IP，沒有自己的IP。所以使用DHCP的**broadcast廣播** ( FFFFFFF ) 送出，拿到自己的IP address，第三層，但DHCP是應用層的，底下是UDP的。
DHCP回傳IP、遮罩、DNS的addr給自己後，你就會有IP
router會知道你的MAC，
Client now has IP address, knows name & DNS server's address, IP address of its first-top router ( 遮罩 ) 

打HTTP的request，所以需要DNS，使用UDP且為應用層。因為DNS server會跨網域，所以會先使用ARP去請求router的MAC addr，收到MAC的router interface過後，就可以對外送出封包。

中間外網跟對方網域的路徑會使用，RIP、OSPF、IS-IS、BGP，到了DNS，就可以到google的IP，所以對google做HTTP的連線，使用TCP，送出過後，google會回傳ACK，就可以確定連線了。

最後送GET的封包過後，等待google的回應後，就會有網頁顯示了！！

## Wireless and Mobile Networks
#### wireless hosts
+ 無線裝置
#### base station 
+ typically connected to wired nerwork
+ 基地台
#### wireless link
會隨著基地台的距離差距跟訊號強弱有極大的關係，也就跟傳輸的速率有關

## Wireless


同一種的SNR ( 雜訊率 ) 越高，BER ( 錯誤率 )就越低
能接受的雜訊越高，錯誤率就會變低

**Hidden terminal problem**
A、B 互相傳資料，C 卻不知道，像是中間有一道牆一樣
**Signal attenuation 信號衰減**
在無線裡面的網路，會因為距離的問題，C 會不知道 A 再向 C 透過 B 傳送。
![[Pasted image 20230104104512.png]]

## CDMA ( Code Division Multiple Access )
頻道透過"碼"去知道訪問誰

sender
receiver 
產生正交碼，就不會產生衝突

## WiFi 802.11 
![[Pasted image 20230104103525.png]]
ISM 的頻譜

#### 架構
AP = access point = base station 家裡那個基地台
所有人都會透過 AP 才會上網


頻譜分為不同頻率的頻道

Passive scanning:
Active scanning:

#### multiple access 多訪問
802.11 沒有碰撞偵測
因為無線無法做 CSMA/CD
所以使用 **CSMA/CA**

#### CSMA/CA
DIFS: 聽了一段時間
他是傳就是傳一整個 Data 出去，和 CD 不同，CD 連續船 Data 出去，聽到碰撞直接不傳
然後 receiver 若是全部都收到，沒有碰撞，就會回傳 ACK

如果聽到忙線中，就會開產生 random backoff time 的時間
等到聽到沒碰撞時才開始計時，結束才會送

雖然盡量避免碰撞，但是有**可能產生**
如果碰撞沒收到 ACK，就會一樣產生延遲時間，等完再傳

#### RTS-CTS
RTS 會發出一個傳輸預約請求，說要送一段時間的資料
如果成功，就會送出訊號，告訴所有人，CTS，有人要傳輸資料
如果失敗，就是預約請求碰撞，就不會回傳成功的 ACK
傳輸成功後，也會告訴所有人 ACK

#### Addressing
一個乙太網路的封包最多可以到 1500 Bytes

## 4G/5G
3G：3rd Generation Partnership Project ( 3GPP )

#### 4G LTE 架構
Base station ( eNode-B ) ( 5G叫做 ng-NB ) 可以解決移動裝置的問題
Mobile device ( UE ) ( User equipment )
IMSI 就像是手機的 sim 卡
radio access network：UE到基地台的區間

管理
HSS ( Home Subscriber Service ) 就是一個資料庫
MME ( Mobile Management Entity )

路由
S-GW ( Serving Gateway )
P-GW ( PDN Gateway )
