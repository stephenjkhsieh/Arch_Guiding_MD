# Arch guiding readable setup MD script
> made by @stephenjkhsieh and @Axelisme

Arch linux 可讀式安裝腳本,可以搭配Axelisme的MD_Executor實現自動安裝與設定。
## MD note
- [Arch_Installation](https://hackmd.io/WA1Lslm3RnG1TC6sQoa7mQ?view)
- [Arch_Software_List](https://hackmd.io/49WeRWbhRd6ztbm49U3zwQ?view)
## Script protocol
### 格式
Special token: #% 和 #@ 和 {}（%和@的數量代表第幾層，{}代表變數插槽）
```
#%[動作]:[特殊描述1],[特殊描述2] [字典]
[指令]
#@
```
#### 格式說明
- #%：起始token
- [_動作_]：增A，刪D，改M，查Q
- [_特殊描述_]：
  - A: m多選，s不詢問用戶，f強制複寫，c需用戶確認才執行，d與資料庫斷開，不取代{}
  - Q: n此層不執行，c需用戶確認才執行，kor多條件key，vor多條件value，d與資料庫斷開，不取代{}
- [_字典_]：狀態以字典結構記錄，以{_'key'_:_'value'_}（或{_'key'_:[_'value'_]}用於多狀態的'key'）
- [_指令_]：reader會執行[_指令_]的內容，當遇到{_'key'_}，則填入用戶'key'內的'value'
- #@：終止token

## Script executor
[MD_Executor](https://github.com/Axelisme/Arch_Setup.git](https://github.com/Axelisme/MD_Executor.git)https://github.com/Axelisme/MD_Executor.git)
