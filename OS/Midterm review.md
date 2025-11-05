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