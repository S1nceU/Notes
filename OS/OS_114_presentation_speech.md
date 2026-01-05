# CH
## Slide 1
各位好，我今天要報告的是 2024 年 Journal of Computer Science 的研究：以情境為基礎評估 CPU 排程演算法效能：FCFS、RR 與 SJF。
這篇論文想解決一個很實際的問題：我們都學過這些經典排程演算法，但很多比較都在「過度理想的靜態案例」下進行；真實世界的行程到達與工作長短其實更像隨機波動，甚至會突然壅塞。
所以作者用模擬器建立 五種不同情境去做壓力測試，並比較 FCFS、SJF，以及 RR 搭配不同時間片（q=1、5、10）在兩個指標上的差異：平均回應時間與平均周轉時間。
接下來我會先把問題與演算法特性講清楚，再講方法與情境，最後看結果並收斂成可帶走的結論。
## Slide 2
這篇研究在談作業系統的 短期排程，也就是 ready queue 裡「下一個誰拿到 CPU」的決策。
短期排程通常要平衡兩種目標：一方面希望 CPU 利用率與吞吐量高；另一方面希望使用者感覺到的等待要低，因此回應時間與周轉時間要小。
所以今天只要你記得一句話就夠：作者用兩個 KPI 來評估——回應時間看互動感，周轉時間看整體完工效率。
## Slide 3
傳統分析常有一個盲點：假設行程到達規律、工作長短可預期，或用很少的案例就下結論。
但真實系統其實像尖峰時段路口：行程可能突然湧入，工作長短混在一起，甚至短時間內高壓壅塞。
因此這篇論文的研究問題很清楚：經典排程在動態負載下，誰穩、誰容易崩？ 以及 RR 那個最關鍵的參數——時間片 q，到底會把效能推向哪裡？
## Slide 4
作者比較三種經典策略：FCFS、RR、SJF。
FCFS 的優點是最直覺、成本低，但缺點是對到達順序非常敏感：長工作早到會把後面全拖住，形成 convoy effect。
RR 的目標是公平與互動性：大家輪流拿 CPU，但代價是 context switching。q 太小切換爆炸，q 太大又會變得像 FCFS。
SJF 很像效率標竿：短工作先做，常能把平均等待壓到最低；但它的前提是能知道或夠準地估計 burst time，現實中不一定做得到。
到這裡你可以先把它當成三種哲學：FCFS 靠順序、RR 靠參數、SJF 靠可預測性。
## Slide 5
我們先深入 FCFS。它的規則就是先到先服務，而且是非搶佔式。
優勢是排程邏輯簡單、額外成本低；但缺點非常典型：如果一個長工作早到，就會把後面的短工作全部卡住，短工作本來可以很快完成，卻因為排隊變成很慢。
這就是 convoy effect：一個「慢車」帶著整隊車一起慢。
因此 FCFS 的效能高度取決於「到達順序」是否友善。
## Slide 6
再看 RR。RR 的設計是 time sharing：每個行程拿到固定時間片就換下一個。
它在互動系統中很常見，因為至少你能保證每個人不會等到天荒地老。
但 RR 的核心不是「輪流」本身，而是 時間片 q。q 很小時，切換頻率極高，CPU 花大量時間在保存/恢復狀態，而不是在真正運算；q 很大時，切換成本下降，但整體行為會越來越像 FCFS，互動性優勢縮水。
所以 RR 的本質是一個「可調的折衷策略」，參數本身就是系統設計的一部分。
## Slide 7
接著是 SJF。SJF 的做法是挑最短 burst time 的工作先做。
在理想假設下——也就是 burst time 已知或可可靠估計——SJF 能把平均等待壓得很低，通常也會帶來更好的回應與周轉表現。
但它的限制也很關鍵：現實世界 burst time 不會「被你精準知道」，通常只能預測；而且如果短工作一直來，長工作可能一直被延後，形成 starvation。
所以你可以把 SJF 看成一個效率標竿或 benchmark：它告訴你「如果你能預測工作長短，理論上可以做到多好」。
## Slide 8
方法上，作者用 Java 寫了一個模擬器，目標是創造更接近真實的波動負載。
每次實驗建立 8 個行程，動態生成 arrival time 與 burst time，然後在相同 workload 下，分別跑 FCFS、SJF，與 RR 的 q=1、5、10。
最後以兩個指標統一比較：平均回應時間與平均周轉時間。
這樣做的好處是：資料一致、對手一致、指標一致，差異就比較能歸因到演算法與參數，而不是「測試不公平」。
## Slide 9
接著是五個 scenarios——這是這篇論文的亮點。
作者不是用一個案例下結論，而是設計五種不同到達與工作長短的組合：有的穩定但偏重、有的晚到且偏短、有的像尖峰壅塞、有的較平衡、也有到達分散且多數偏短但仍包含少量長工作。
這樣的設計很重要，因為排程演算法常常不是「本質誰更好」，而是「誰更適合某一種負載結構」。
所以等一下我們看圖表時，重點不是背數字，而是看「情境改變後，排名怎麼翻轉」。
## Slide 10
這頁把五個情境的測試目的講得更清楚：
有些情境是在放大 FCFS 的順序敏感與 convoy effect；有些情境讓短工作占多數，讓 SJF 的優勢更明顯；而所有情境都用來觀察 RR 的 q 如何在「反應性」與「切換成本」之間改變效能。
到這裡我們其實已經把「演算法的弱點」與「情境要測什麼」對齊了。接下來只要看結果，就能理解為什麼會出現那樣的曲線或柱狀圖。
## Slide 11
這張表列的是每個 scenario 的 arrival time 與 burst time。你可以把它當成整份實驗的輸入地圖。
從這裡你會看到：有些情境一開始到達密集、有些情境到達較晚且更分散；burst time 也有情境偏短、有情境混合長短、甚至有少量極長作業作為 outlier。
這些差異會直接影響排程：只要你把「長工作早到」這件事放進來，FCFS 就很容易被拖；短工作比例提高，SJF 就會更像清道夫；RR 則會因為 q 改變切換頻率而改變效率。
因此看結果前先看表格，可以避免我們把差異錯怪到演算法身上。
## Slide 12
現在看 平均回應時間。回應時間是「到達後多久才第一次拿到 CPU」。
三句話抓重點：
第一，SJF 通常最低，因為短工作會更快被啟動。
第二，RR 的 q=1 通常最糟，原因不是 RR 的公平性有問題，而是切換成本太大。
第三，RR 的 q=5 或 10 會顯著改善，因為切換變少、有效運算比例提高；而 FCFS 在某些平衡情境下還可以，但遇到壅塞或長工作早到就會惡化。
所以這頁不要念數字，講趨勢就能讓觀眾抓到主線。
## Slide 13
把回應時間的「為什麼」說清楚：
SJF 的穩定優勢來自它讓短工作很快開始執行，佇列不容易堆高，因此平均回應自然下降。
FCFS 的變動很大，原因是它完全依賴到達順序：只要前面是長作業，後面就算是短作業也必須等待，尤其在壅塞型情境會更明顯。
RR 的關鍵是 q：q=1 時 CPU 忙著切換，導致大家第一次拿到 CPU 的時間反而變長；q 增大後切換下降，回應時間就改善並接近 FCFS 行為。
這也把我們前面講的哲學對起來：RR 的公平是要付出 overhead 的。
## Slide 14
這頁專門補 RR 的機制：context switch 需要保存與恢復狀態，這些動作不產生實際運算，但會吃 CPU 時間。
當 q 極小，切換次數暴增，CPU 的有效工作比例下降，overhead 反而主導效能，導致回應與周轉都變差。
當 q 變大，切換變少，overhead 降低，效能改善；但同時 RR 的互動性優勢會縮小，因為它更像 FCFS。
因此 RR 的精髓就是在這兩端之間找甜蜜點。
## Slide 15
接下來看 平均周轉時間，它是從到達到完成的總耗時，更偏整體完工效率。
這個指標對「長作業拖累」更敏感：如果長作業把佇列堵住，後面所有作業的完工時間都被推遲，平均周轉就會變差。
整體趨勢依然類似：SJF 多半最好；FCFS 在長作業早到會惡化；RR 則取決於 q 是否把切換成本壓住。
所以周轉時間這局看的不是誰先被服務，而是誰能把整體「做完得更快」。
## Slide 16
把原因說得更明確：
FCFS 最大的拖累是順序造成的堵塞——長作業早到就把整隊完工時間拉長。
RR 的拖累是切換成本——q 太小時，CPU 做了很多切換而不是完成工作，周轉自然變差。
SJF 在這組模擬條件下同時避開兩種拖累：它把短作業快速完成、降低佇列積壓，因此平均周轉通常最漂亮。
但要提醒觀眾：這是在 burst time 可用於排序或可預測的模擬設定下的結果。
## Slide 17
這頁是 RR 的核心心智模型：Quantum Dilemma。
q 小：互動性理論上更好，但切換成本爆炸；q 大：成本下降但越像 FCFS。
所以 RR 在工程上常用，是因為它公平且可部署，但它永遠是一個參數化策略：你必須選擇適當的 q 才能達成折衷。
以這篇研究的結果來看，q=1 幾乎是最差設定，q 提升到 5 或 10 會顯著改善，代表「適中時間片」是比較務實的設計方向。
## Slide 18
我們用總結矩陣收斂：
SJF 是效率標竿，在模擬條件下最穩定；FCFS 最簡單、成本低，但對順序非常敏感；RR 是實務折衷，但必須調參，尤其 q 不能太小。
這也再次呼應論文的主張：沒有單一演算法能在所有情境下完美，重點是「匹配工作負載」。
## Slide 19
最後做現實檢查：SJF 很強，但它依賴 burst time 的可得性或可預測性，而且可能造成 starvation。
所以在真實作業系統中，SJF 常被當成理想 benchmark，而 RR 因為不需要知道工作長短、能保證公平，往往更符合一般用途需求。
因此如果你問「現實會怎麼選？」答案通常是：RR 這類可部署策略，再搭配合理的 q 與其他機制去平衡效率與公平。
## Slide 20
結論是：用 scenario-based 方法比較排程，才能看到演算法在不同負載下的真實表現與脆弱點。
這篇研究指出 SJF 在理想條件下最強、FCFS 容易被長作業早到拖垮、RR 需要合理的 quantum 才能避免切換成本主導。
未來方向很自然：更自適應的排程，例如動態調整 quantum、用資料或機器學習預測負載特性，甚至做 hybrid scheduling 結合多種策略的優點。
我的報告到這邊，謝謝大家

# EN Script (Simple English, Readable)
## Slide 1 — Title
Hello everyone. Today I will present a 2024 paper from the Journal of Computer Science.
The title is “Performance Assessment of CPU Scheduling Algorithms: A Scenario-Based Approach with FCFS, RR, and SJF.”
This study compares three classic CPU schedulers: FCFS, Round Robin, and SJF.
The key idea is simple: real workloads are not clean and predictable, so the authors test these algorithms under five stress-test scenarios.
They measure performance with two metrics: response time and turnaround time.

## Slide 2 — The Context: Short-Term Scheduling
This paper focuses on short-term scheduling.
That means: the OS looks at the ready queue and decides which process(ㄆㄨㄚ ses) gets the CPU next.
The goals are shown on the slide:
We want to maximize CPU utilization and throughput, and minimize response time and turnaround time.

## Slide 3 — The Core Problem
Traditional analysis often assumes a static and predictable system.
But the real world is chaotic. Workloads can change fast, and congestion can happen suddenly.
So the main question is:
When the workload changes, which algorithm still performs well, and which one breaks down?

## Slide 4 — The Contenders
The paper compares three schedulers.

First, FCFS. It is a simple FIFO queue.
Its weakness is the convoy effect(ㄜfect): short jobs can get stuck behind a long job.

Second, Round Robin. Each process gets a fixed time quantum.
Its weakness is that performance depends a lot on the quantum size.

Third, SJF. It runs the shortest job first.
Its weakness is that it needs knowing burst time, and long jobs may starve.

## Slide 5 — Deep Dive: FCFS
FCFS is first come, first served, and it is non-preemptive.
So one CPU-bound process can **monopolize[mㄜ no po lize]** the CPU for a long time.
If a huge job arrives first, then many short I/O jobs must wait.
That is the convoy康voy effect, and it increases waiting time a lot.

## Slide 6 — Deep Dive: Round Robin
Round Robin is a trade-off controlled by the time quantum.

If the quantum is small, the advantage is high responsiveness for interactive use.
But the disadvantage is too many context switches. The CPU may spend more time switching than working.

If the quantum is large, the advantage is low overhead and better **efficiency[ㄜfioncy]**.
But the disadvantage is it behaves more like FCFS, and responsiveness becomes worse.

## Slide 7 — Deep Dive: SJF
SJF has a major **limitation**. It assumes: “the system must know future burst times.”
In real OS environments, **exact[依z柯特]** prediction is hard, so pure SJF is not easy to deploy.

But under ideal conditions, SJF is very strong.
It is known for minimum average waiting and turnaround time, and it clears short jobs quickly.

## Slide 8 — Methodology: The Simulation
The authors built a custom Java simulation to test dynamic conditions.
In each run, they create 8 processes.
They use a function called Random Time generator to generate random arrival times and burst times.
Then they test FCFS, SJF, and RR with different quantum values: 1, 5, and 10.
Finally, they log two metrics: response time and turnaround time.

## Slide 9 — The 5 Stress Test Scenarios
Here are the five scenarios used for stress testing.

Scenario 1: Steady & Heavy. Steady arrival, long burst times. It tests **endurance[in du rance]**.
Scenario 2: Quick & Staggered. Late start, mostly short tasks. It tests rapid turnover.
Scenario 3: Early Congestion. Most processes arrive immediately, with high burst variance. This is the hardest case.
Scenario 4: Balanced. Arrival and burst times are more even. This is the baseline.
Scenario 5: Delayed & Mostly Short. Later arrivals, mostly short tasks, but with a few long bursts.

## Slide 10 — Scenario Data Detail (Arrival, Burst)
This table summarizes the objective of each scenario.
Scenario 1 tests handling of mixed long and short jobs under steady arrival.
Scenario 2 focuses on responsiveness under light load, since jobs start late and are short.
Scenario 3 is the key stress test—the slide states it is the primary failure point for FCFS, because **congestion[肯jackion]** plus long jobs early is the worst case for **convoy[康voy]** effect.
Scenario 4 is the balanced standard load control.
Scenario 5 tests idle performance when arrivals are **sparse[斯霸爾斯]** and many bursts are short.
`Scenario 3 has high concurrency and long burst times.`
`So we should expect Scenario 3 to create the most congestion, and to punish weak scheduling choices.`

## Slide 11 — Scenario Data Detail Table
This table is basically the input map for the whole experiment.
It tells us when each process arrives, and how long it runs.
So when we see results later, we can connect them back to the workload pattern—especially why Scenario 3 is so difficult.

## Slide 12 — Results: Average Response Time
Now we look at average response time, meaning: how long a process waits until it gets CPU the first time.

Main points from the slide:
First, SJF is consistently the lowest across all five scenarios.
Second, RR with Quantum = 1 performs poorly because context switching overhead is too high.
Third, RR with Quantum = 5 or 10 improves, and becomes more FCFS-like, especially when bursts are short-to-moderate.
Also, Scenario 3 (Congestion) makes performance drop, because long jobs arrive early and delay everyone.

## Slide 13 — Response Time Interpretation
So why does response time look like this?
SJF wins because it starts short jobs earlier, so the queue clears faster.
FCFS can look okay in balanced cases, but it depends on arrival order. If a long job comes first, response time jumps.
For RR, the quantum decides everything: too small means too much switching, too large means it behaves like FCFS.

## Slide 14 — Analysis: Context Switching Cost
This slide answers: Why did Q=1 fail?
With Quantum = 1, the CPU switches tasks constantly.
The overhead accumulates, so response time becomes much worse than FCFS or SJF.
The key message on the slide is:
Time spent switching can become larger than time spent processing.

## Slide 15 — Results: Average Turnaround Time
Next is average turnaround time, meaning: from arrival until the process finishes.
The slide summary is: SJF maintains the best performance across all scenarios.
Scenario 5 is the best case because burst times are very short.
Scenario 3 is the worst case because early long processes block the queue.
For RR, small quantum is **inefficient[in a fion t]**, and large quantum reduces context switches. In Scenario 3, a larger quantum handles long bursts better and can match FCFS efficiency.

## Slide 16 — Turnaround Time Interpretation
Turnaround time cares a lot about one thing: how fast the whole system finishes jobs.
SJF helps by finishing many short jobs quickly, so fewer jobs stay in the system for a long time.
FCFS suffers most when a long job comes early. That pushes back the finish time for everyone.
RR again depends on quantum: if quantum is too small, overhead delays completion.

## Slide 17 — Deep Dive: The Quantum Dilemma
This slide gives the main conclusion for RR: there is no magic number.
The best quantum depends on the workload.
If Q = 1, you get high responsiveness, but also high overhead.
If Q = 10, overhead is low, but responsiveness is low and it becomes like FCFS.

## Slide 18 — Summary Matrix
This matrix summarizes the takeaways.

FCFS: “Simple but Brittle.” It can be okay in balanced loads, but it is very sensitive to arrival order, and it **collapses[call lap ses]** under congestion like Scenario 3.

Round Robin: “The Great Compromise.” It is the standard for fairness, but performance is dictated by quantum size, so it needs tuning.

SJF: “**Theoretical[th o re ti cal]** Champion.” It wins the metrics, but it depends on predicting burst times, so it is hard to deploy in general-purpose OS.

## Slide 19 — Reality Check: Theory vs. Practice
This slide is the **reality[re 阿 le ty]** check.
Yes, SJF consistently won the simulation metrics.
But SJF assumes zero-error prediction of burst times, and it ignores the cost of starvation for long jobs.
So in real life, SJF is more like an **ideal ceiling**, not a direct solution.

Round Robin did not win every metric, but it is practical:
It needs no **prior[py er]** knowledge of jobs, and it **guarantees[gay ron tee se]** fairness with no starvation.
With an optimized quantum, like Q = 5, it can give the best balance for real systems.

## Slide 20 — Conclusion & Future Work
So the final message is the quote on the slide:
“The ideal scheduling strategy depends on the **specific[s b cfic]** requirements of the operating environment.”
In other words, the best scheduler depends on the workload and the system goal.
That is the end of my presentation. Thank you.
