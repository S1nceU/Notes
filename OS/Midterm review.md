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
8. A system is executing processes with average CPU burst time T ms, and there is a context switch every N burst. What is the maximum allowable context switch time S ms to keep the overhead less than 5%? (T is 100 and N should be 2, and please truncate the answer to two decimal places) 
	- $Overhead = \frac{S}{N * T+S} < 0.05 \rightarrow S < \frac{0.05}{0.95}NT$
	- $S < \frac{0.05}{0.95}*200 = \frac{200}{19}ms$
9. In the pseudocode, the purpose of the parent process using wait(NULL) is:
	```
	  else { /*parent process*/
		  /* prent will wait for the child to 
		  wait (NULL);
		  printf("Child Complete");
	  }
	  ```
  - To wait for the child process to terminate and reclaim its status, preventing hte child process from becoming a zombie
10. What is a "socket"? How is it applied in client-serverr system communication?
	- Socket 是進程通訊的端點
	- **Server**：建立 socket → 綁定到服務 **port** → 監聽並接受連線；之後以該連線上的兩端 socket 讀寫資料。
	- **Client**：建立 socket → 連到伺服器的 `IP:port` → 與伺服器成對通訊（TCP/UDP 皆可）。
## Ch4
1. In an operating system, when a process receives a signal, the system must deliver it to some thread for handling. Which of the following approaches is the simplest and most convenient way to implement signal handling?
	- Designate a single thread to act as the agent, responsible for handling all signals.
2. What are the main benefits of multithreaded programming?
	+ Responsiveness : 反應好，當其中有執行續被中斷，則其他執行緒則會接上使用 CPU
	+ Resource sharing : 有共同的位址空間
	+ Economy : cost 比 process 還要高，process 需要做其餘轉換跟記憶體共享
	+ Scalability : 可以有多個執行緒在 process 上，擴展性充足
3. A thread function in Windows C Program is conventionally called by runner, yes or no? Why and why not?
	-  NO
	- 名稱是自訂的，thread function 是被 OS 建立的並呼叫
4. If the child process created by fork() immediately calls exec(), what happens to the threads?
	- child process 呼叫 exec() 時，整個 process 會被新的程式所取代，原本 fork() 的 thread 會無法使用，新程式會以一個新的 thread 展開
	- fork() 會複製一個新的 thread 或是一個新的 process
	- exec() 會取代整個 thread 以及整個 process 位址空間
5. In multithreading, what is the common way to create way to create a new thread?
	- Using a Thread class or threading library
	- 用 Thread 函式庫去建立一個 Thread
6. What are the three common multithreading models that relate user threads to kernel threads?
	+ Many-to-One 
	+ One-to-One
	+ Many-to-Many
7. Explain the differences in the usage of registers and stacks between a single-threaded process a multithreaded process, and clarify why they are designed this way.
	- Single thread
		- 只有 **一組暫存器狀態**（含 PC）與 **一個執行緒堆疊（stack）** 隨程式逐步執行。
		- 使用所有的暫存器和資料
	- Multi thread
		- 每個執行緒各自擁有一組暫存器集合（含 PC）與自己的堆疊，同一行程內的緒共享程式碼、全域資料與堆積（heap）。
		- 分別暫存器使用，並使用同一程式碼以及資料
8. Thread and pthread are often mentinoed in OS. Please explain their relationship and why pthread is often emphaized in Linux systems.
	- Pthread is 規範、規格, use to create and manage thread.
9. In the M2M thread model, what is the mechanism by which the kernel can notify the user-level thread library about resource availability? Describe this method concisely. 
	- 使用 Scheduler Activations 進行 Upcall，當 thread 有事件發生時，Kernel 會用 upcall handler 發出通知
	- upcall hanler 允許應用管理 kernel thread 的數量或是狀況
## Ch5
1. In a computer system simulation, when the varibale Clock increases, what action does the simulator perform?
	- Clock 是模擬器裡的模擬時間，Clock 進行時，會更新系統狀態去反應裝置以及排程動態
2. Which of the following is NOT a common optimization criterion for scheduling algorithm?
	- - CPU utilization
		- 為了讓 CPU 盡可能忙碌
	- Throughtput
		- 每單位時間內平均完成 processes 的工作量
	- Turnaround time
		- 整個過程開始到結束的時間
		- 各種 state 到 terminate
	- Waiting time
		- 花費在 ready queue 的時間
	- Response time
		- 從開始到第一次執行的時間
3. What is memory stall?
	- 當 thread 執行的時候，讀寫記憶體或是指令時會有一個短時間的延遲
4. What is the potenital isssue when using Priority Scheduling, and what is the solution to it?
	- Priority Scheduling 會造成 Starvation
	- 可以用 agin，隨時間增加 process 的優先權，解決 starvation
5. 示圖
	Q: ![[Pasted image 20251106163610.png|500]]
	A:
	![[Pasted image 20251106163642.png]]
	![[Pasted image 20251106163654.png]]
	
6. In CPU scheduling, what is the main difference between a Multilevel Queue and a Multilevel Feedback Queue?
	- MLQ 的分層是固定的，無法移動 process 的位置
	- MLFQ 可以根據規則使得 process 可以在不同的Queue動態調整優先權
7. In multiprocessor scheduling, please explain the main difference between "soft affinity" and "hard arrinity".
	- soft affinity : 任何的 process 都可以被所有的 processor 處理
	- hard affinity : 只能被同一 processor 處理
	
	- Hard affinity: 把 process 綁定指令 affinity maksk，使得只能在這個 processor 處理
	- Soft affinity: 則可以遷移 process 給其他的 processor 做，使得平衡負載
8. Explain the difference between "preemptive" and "non-preemptive" CPU scheduling, and provide one example algorithm for each.
	- Preemptive : 在 CPU 執行時，可以強制搶斷 CPU 的執行權，接上其他的 process，像是 RR、SRTF
	- Non-preemptive : 其他的 process 需要等待上一個 process 做完或是阻塞才會換成自己執行，像是 FCFS
9. In RR scheduling system, explain the difference, when time quantum (q) is extremly large and extremely small.
	- Large q : 近似 FCFS，回應慢，但 context switch 的消耗低
	- Small q : 形成頻繁搶斷，context switch 會很高，導致 overhead 暴增
10. Convey Effect
	- Convoy effect＝**一個慢／長工作的行程拖住整隊**，其他行程（多半是 I/O-bound）都在等共享資源，整體效能下滑。
## Ch6
1. Advantage or feature of Peterson's Solution?
	- 保證 **Mutual Exclusion**、**Progress**、**Bounded Waiting**
	- 沒有硬體支援
	- 假設 load 以及 store 原子性
2. What is the purpose of the Entry Section in the critical section?
	- 用於控制許可並確保只有一個 process 可以進入 crtical section
	- 在進入 critical section 前，**執行存取協定以「請求並取得進入許可」**；若尚未安全可進入，就在此等待，直到協定保證可進入（符合 **Mutual Exclusion / Progress / Bounded Waiting** 的設計）。
3. test_and_set
	![[Pasted image 20251026194800.png|300]]
4. Which of the three properties - Mutual Exclsion, Progress, or Bounded Waiting - does the implementation of test_and_set() fail to satisfy? Please explain the solution in one sentence.
	- 無法保證 Bounded waiting
	- 單純 `test_and_set` 自旋鎖只能保證 Mutual Exclsion，無法保證每次請求後等待次數有上界（可能 starvation）。
5. Two processes, P1 and P2, both update a shared variable count. P1 executes count++, and P2 executes count--. Explain what happens if these two instructions execute concurrently without synchronization.
	![[Pasted image 20251026162214.png|300]]
	![[Pasted image 20251106173039.png|500]]
6. What is the atomic instructions?
	- 是一個不能被搶斷的操作，確保操作可以一次就執行完成，不會有中斷，可以用來實現鎖或是 CS
7. Use a binary semaphore to force the execution order: P1's S1 must run before P2's S2. Write minimal pseudocode and state the initial value of the semaphore with a brief reason.
	```c
	//P1
	S1;
	signal(synch); // 確保 S1 可以先執行，並通知 P2 可以執行 S2
	
	//P2
	wait(synch);   // 等待完 S1 執行完，才會往下跑 S2
	S2;
	```

8. wait() 以及 signal() 的操作為什麼必須在 CS 中執行?
	- (C) 為了確保對信號量的**內部狀態（如計數器）**所做的修改是**原子性**的，從而避免**競態條件**。
9. For avoid Busy waiting, when a thread call wait() and finds the resource is unavailable, what is the most critical next operation?
	- 避免 Busy Waiting 的信號量實作中，`wait()` 會把呼叫 thread **加入信號量的等待佇列並阻塞（block）**；之後 `signal()` 會**從佇列喚醒一個緒並送回就緒佇列**。
10. What is the difference between a counting semaphore and a binary semaphore?
	- 用來**計數可用資源數**，可同時放行多個執行緒的資源
	- 只有兩個狀態，控制一次只能一個進入 CS 做操作