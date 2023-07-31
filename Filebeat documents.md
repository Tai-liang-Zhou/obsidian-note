### [what is harvester]([How Filebeat works | Filebeat Reference [8.9] | Elastic](https://www.elastic.co/guide/en/beats/filebeat/current/how-filebeat-works.html#harvester)):
If a file is removed or renamed while it’s being harvested, Filebeat continues to read the file. This has the side effect that the space on your disk is reserved until the harvester closes. By default, Filebeat keeps the file open until [`close_inactive`](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-input-log.html#filebeat-input-log-close-inactive "close_inactive") is reached.

### Trouble shooting
[Open file handlers cause issues with Windows file rotation | Filebeat Reference [8.9] | Elastic](https://www.elastic.co/guide/en/beats/filebeat/current/windows-file-rotation.html)
[Filebeat keeps open file handlers of deleted files for a long time | Filebeat Reference [8.9] | Elastic](https://www.elastic.co/guide/en/beats/filebeat/current/faq-deleted-files-are-not-freed.html#faq-deleted-files-are-not-freed)

### [filebeat input log close options]([Log input | Filebeat Reference [8.9] | Elastic](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-input-log.html#filebeat-input-log-close-options))

Used to close the harvester after a certain criteria or time.

If a file is updated after the harvester is closed, the file will be picked up again after `scan_frequency` has elapsed.


##### `close_inactive`

Filebeat closes the file handle if a file has not been harvested for the specified duration.
	設定某段時間 log 不再更新時關閉讀取

If the closed file changes again, a new harvester is started and the latest changes will be picked up after `scan_frequency` has elapsed.
	如果 log 再度更新了，filebeat 會設定 scan_frequency 的頻率下再次開啟讀取檔案

建議設定時間大於 ap 更新 log 的時間
如果是 event trigger ap, ex: MQ or cronjob 視情況調整

可以針對不同 AP 來進行 config 設定

如果設定太小可能會造成 filebeat 沒有辦法及時更新最新一筆 log
時間的判斷不再於檔案更改時間，而是 harvester 讀取最後一行之後的累積時間
For example, if `close_inactive` is set to 5 minutes, the countdown for the 5 minutes starts after the harvester reads the last line of the file.

##### `close_renamed`

當 file 被 rename 的時候 filebeat 將會關閉檔案。
通常發生在 log rotate
Filebeat closes the file handler when a file is renamed. This happens, for example, when rotating files.

##### `close_removed`

 Filebeat closes the harvester when a file is removed.
 如果沒有設定此選項，並 `close_inactive` 也沒有觸發 close，檔案會一直保持開啟

##### `close_eof`
data loss is a potential side effect - 有可能會遺失資料
通常運用於一次只寫一份log並內容之後也不再更新的狀況
This option is `disabled` by default.

##### `close_timeout`
data loss is a potential side effect - 有可能會遺失資料
為 harvester 設定一個固定時間，時間過後關閉檔案
如果 log 持續更新，log 將會在下次 `scan_frequency` 後被重啟
This option can be useful for older log files when you want to spend only a predefined amount of time on the files.
This option is set to 0 by default which means it is `disabled`.

`close_timeout` and `ignore_older` 設定為相同數值，檔案被關閉之後將不會再被打開