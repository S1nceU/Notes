## Minimax Decision
最大話與最小化利益
#### Determining MV Values
定義每個步驟的分數
MAX就選最大的
MIN就選最小的

## Alpha-Beta Pruning
$min(3,x,y)$ -> 答案必定 <= 3
$max(5,min(3,x,y,...))$ -> 答案必定是5
所以已經知道結果了，就不用繼續搜尋下去
![[Pasted image 20221122163908.png]]
#### Move Ordering for Alpha-Beta Search 
搜尋順序重要

## Heuristic Alpha-Beta Search

