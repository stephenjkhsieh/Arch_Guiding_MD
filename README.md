# Arch guiding readable setup MD script 
>[name=Stephen JK Hsieh][name=Axelisme]
Arch linux 可讀式安裝腳本,可以搭配Axelisme的MD_Executor實現自動安裝與設定。

## Script protocol
針對installer的special token: #% 和 #@ 和 {}（%和@的數量代表第幾層，{}代表變數插槽）

###格式
#%[動作]:[特殊描述1]?,[特殊描述2]? [字典]
[指令]
#@

####格式說明
#%：起始token
[動作]：增A，刪D，改M，查Q
[特殊描述]：
　　A: m多選，s不詢問用戶，f強制複寫，c需用戶確認才執行，d與資料庫斷開，不在{}填入資訊
　　Q: n此層不執行，c需用戶確認才執行，kor多條件key，vor多條件value，d與資料庫斷開，不在{}填入資訊
[字典]：狀態以字典結構記錄，以{'key':'value'}（或{'key':['value']}用於多狀態的key）
[指令]：reader會執行[指令]的內容，當遇到{'key'}，則填入用戶'key'內的'value'
#@：終止token

## Script executor
MD_Executor prepository:
https://github.com/Axelisme/Arch_Setup.git](https://github.com/Axelisme/MD_Executor.git)https://github.com/Axelisme/MD_Executor.git
