## Memory Access Architecture
UMA 
+ 多用在 SMP
+ 每個 CPU 速度是相同的
+ 用同一個 memory
+ 在 CPU 數量增加時，速度就會相對變慢

NUMA
+ 每個 CPU 都有個別的 memory 
+ 所以每個 CPU 的速度都會快
+ 但在 CPU 互相溝通時，有訪問路線

