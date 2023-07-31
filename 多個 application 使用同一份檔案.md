多個程式嘗試讀取同一份 log 可能導致問題的幾個原因:

1. **文件系統的鎖定機制** (Race condition)：
	當一個程式正在寫入日誌時，文件系統可能會鎖定該文件，以防止其他程式同時進行讀取或寫入。
2. **併發寫入的衝突** (Blocking)：
	如果多個程式同時嘗試寫入同文件，可能會發生寫入衝突。
3. Log is rotated or renamed:
	如果 ap 一直開啟檔案可能會造成 log rename or remove 問題。