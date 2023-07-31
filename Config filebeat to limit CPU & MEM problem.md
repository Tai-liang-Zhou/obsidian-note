

### CPU - 2

[Filebeat is using too much CPU](https://www.elastic.co/guide/en/beats/filebeat/7.17/filebeat-cpu.html#filebeat-cpu)
Filebeat might be configured to scan for files too frequently. Check the setting for `scan_frequency` in the `filebeat.yml` config file. Setting `scan_frequency` to less than 1s may cause Filebeat to scan the disk in a tight loop.

[`harvester_limit`](https://www.elastic.co/guide/en/beats/filebeat/7.17/filebeat-input-log.html#filebeat-input-log-harvester-limit)

The `harvester_limit` option limits the number of harvesters that are started in parallel for one input. This directly relates to the maximum number of file handlers that are opened. The default for `harvester_limit` is 0, which means there is no limit.

[`rospector.scanner.check_interval`](https://www.elastic.co/guide/en/beats/filebeat/7.17/filebeat-input-filestream.html#filebeat-input-filestream-scan-frequency)
How often Filebeat checks for new files in the paths that are specified for harvesting. For example, if you specify a glob like `/var/log/*`, the directory is scanned for files using the frequency specified by `check_interval`. Specify 1s to scan the directory as frequently as possible without causing Filebeat to scan too frequently. We do not recommend to set this value `<1s`.

If you require log lines to be sent in near real time do not use a very low `check_interval` but adjust `close.on_state_change.inactive` so the file handler stays open and constantly polls your files.


### MEM - 1

##### [`buffer_size`]([filestream input | Filebeat Reference [7.17] | Elastic](https://www.elastic.co/guide/en/beats/filebeat/7.17/filebeat-input-filestream.html#_buffer_size))
The size in bytes of the buffer that each harvester uses when fetching a file. The default is 16384.

[`bulk_max_size`]([Configure the Elasticsearch output | Filebeat Reference [7.17] | Elastic](https://www.elastic.co/guide/en/beats/filebeat/7.17/elasticsearch-output.html#_bulk_max_size))
The maximum number of events to bulk in a single Elasticsearch bulk API index request. The default is 50.

Events can be collected into batches. Filebeat will split batches larger than `bulk_max_size` into multiple batches.

Specifying a larger batch size can improve performance by lowering the overhead of sending events. However big batch sizes can also increase processing times, which might result in API errors, killed connections, timed-out publishing requests, and, ultimately, lower throughput.