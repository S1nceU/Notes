Informaiton preseveration

Measures of quality
+ Making sure attribute *semantics* are clear
+ Reducing *redundant information* in tuples
+ Reducing NULL values in tuples
+ Disallowing possibilty of generating *spurious tuples* ( 假設的資料 )

非正式指引
實體跟屬性不要放在一起，把外鍵連到的東西放在一個 entity，這樣表格的table會超級大，這樣不好

Information is stored redundant
+ 浪費儲存空間，員工的部門會一直重複
+ 會造成更新異常
	+ 可能會造成查詢資料不一致
	+ 可能有不能新增Proj#為null的員工，因為他是PK
	+ 要刪除Proj#，這樣員工就會不見
	+ 更新時，只要改一個部門資訊，就全部都要改

指引2:
+ Design a schema thatdoes *not* suffer from the insertion, deletion and update anomalies
+ if

Null Values in Tuples
+ 浪費空間
+ 不知道其中 NULL 的意思

指引3:
+ Avoid placing att
+ 若有多NULL，就拉出到新表格

Bad design -> erroneous results for certaion JOIN operations
The "*lossless join*" property 
指引4:


FD constraints
An FD is a property of the attrubutes in the schema R
The 