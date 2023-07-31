Log 是具有分析價值的 data:
* error message 
* case handle
* data process time
* user self service tool

增加 Log 分析效率:
	有一次一位主管需要我分析 MES Service API 每天 access 的 count 數統計
	需要用 python 寫
	開出一個 sprint 的 user story 給我
	但是我想起 mes service log 其實有透過 logstash 上傳
	用約一個小時的時間，我完成了 mes 總共 6 台 server 的 log 分析
	還能製作出長條圖圖表讓主管清楚分析 每隻 API 的使用率

以往我們要分析 log 
	1. 需要寫程式來 ssh 連線 
	2. 想 log parser 的邏輯 
	3. 每一次的 deploy 都需要很長時間的 change 

我們可以透過 filebeat 在 kibana 分析 log，還可以使用各種 kibana 的視覺化工具
所有各廠多台 server 的 log 都可以從一座 elastic 搜尋分析
只需要下 KQL 就可以輕易抓出想要分析的資料

減少不同 log parser 的風險，多種不同語言的 log parser 可能會造成 log 卡住的狀況，可以從 elastic 上面做第一手資料的搜尋
之後可以從 mtail 來處理

