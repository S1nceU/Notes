## CH1、CH2
1. Which of the following are major types of system calls in an OS?
	1. Process control
	2. File management
	3. Device management
	4. Information maintenance
	5. Communications
	6. Protection
2. In the Interrupt Timeline, how does it show the parallel operation of CPU and I/O?
	- ![[Pasted image 20251105141204.png]]
3. What is the main difference between Multiprogramming and Time-sharing?
	1. Multiprogramming : 透過同時常駐多個工作、遇 I/O 即切換，來**提高 CPU 使用率**
	2. Time-sharing (Multitasking) : 以極短時間片頻繁切換，讓使用者在程式執行時可即時互動
4. Multiprocessor environment must provide __ in hardware such that all CPUs have tthe most recent value in their cache.
	1. cache coherency
5. What is Operating System Definition and Goal?
	- Def
		- OS 是介於使用者與硬體之間的程式；同時扮演**資源配置者**與**控制程式**的角色。
	- Goal
		- 執行使用者程式、提升使用便利性、並有效率地利用硬體
6. Is the counter in a single thread the same as the program counter (PC) in the CPU? Why or why not?
	- 不是
	- 單一 thread 有自己的 program counter，被排程後 ，CPU的PC暫存器會載入 thread 的 PC，所以兩個是不同的實體
7. Why do operating systems distinguish between "User Mode" and "Kernel Mode"? What is the importance of this design for system stability and security? Furthermore, please explain the specific mechanism for switching from User Mode to Kernel Mode.
	1. 透過 OS 介面**受控存取**資源，提升**穩定性**與**安全性**，避免使用者修改到 OS 的核心程式導致 fatal error 或是破壞硬體
	2. 可透過系統呼叫或是中斷進入 Kernel mode，進行核心的程式
8. Which statement about system calls and APIs is correct?
	1. APIs typically wrap one or mote system calls to provide a convenient interface
9. You are playing a shooting game and click the left mouse button to fire. From the moment you click until the bullet is fired, what does the computer do to make this happen smoothly?
	- 當使用者滑鼠點擊時，會產生一個 interrupt，CPU 處理中斷後回傳訊號給遊戲
10. Main memory is designed as sequential access so that it can store data in an ordderly manner. The statement above is T or F? Explain answer.
	- F
	- Main memory 是隨機存取的，Sequential access 是磁帶裝置的特性，不是 main memory 的設計
## CH3 
1. In the message passing of the two models of IPC in tthe cooperating processes, when process A needs to passs a message to process B, is it transmitted directly? yes or no. If the answer is no please explain message passing.
	- No
	- Message passing 中，A $\rightarrow$ B 需要先建立通訊連結，再用 send 和 receive 交換訊息
	- A 不會「直接」把資料塞進 B；是透過 OS Kernal 所建立的連結與 mailbox或是buffer，並以 **send/receive** 在核心中轉遞並交付給 B。
2.  Process communication is carried out through IPC, which can be implemented using either shared memory or message passing. What are the advantages and drawbacks of these two communication methods
	- **共享記憶體（Shared memory）**  
		- 優點：速度快，適合大量資料交換。  
		- 缺點：在讀寫等操作中可能產生同步問題。
	- **訊息傳遞（Message passing）**  
		- 優點：由核心控制通訊，可避免資料同步問題。  
		- 缺點：資料傳輸需要系統呼叫，因此相對較慢。
	- ![[Pasted image 20250922155933.png|500]]
3. Draw the process state transition diagram including the states: New, Ready, Running, Waiting, and Terminated. Label the key transition events.
	- ![[Pasted image 20251105191838.png]]
4. Which of the following are stored in a Process Control Block (PCB)?
	1. Process state
	2. Program counter 
	3. CPU registers
	4. CPU scheduling information
	5. Accounting information
	6. I/O status information
5. In message passing communication, which of the following is correct?
	1. Non-blocking receive may return a null message if no message is available
6. Simply example what is context switch?
	1. 當 CPU 由一個行程/執行緒切到另一個時，**把舊的 CPU 狀態存入其 PCB，載入新的 PCB 狀態再繼續跑**；切換期間屬**額外開銷（overhead）**，常由計時器中斷、系統呼叫或 I/O 事件觸發。
	2. 把正在跑的程式 A 的「進度」（暫存器、PC 等）先存起來，再把程式 B 的進度載回來，然後改跑 B。通常是**計時器到點、系統呼叫、或 I/O 完成**時發生
7. The primary functions of the long-term scheduler and short-term scheduler?
	- Short-term scheduler ( CPU scheduler ) : 選擇 Ready queue 中的 process 給 CPU 做
	- Long-term scheduler ( job scheduler ) : 為了讓 CPU 使用最高效，所以會一直安排工作到 ready queue 上，調控系統中 process 的數量 ( degree of multiprogramming )
	- Short term 是決定那個 process 選擇給 CPU 做運算，Long term 是決定哪個 process 進入 ready queue