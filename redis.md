### Redis性能测试
#### 一、测试get和set性能
- 10字节
```Shell
====== SET ======
  100000 requests completed in 2.88 seconds
  50 parallel clients
  10 bytes payload
  
34758.43 requests per second

====== GET ======
  100000 requests completed in 2.88 seconds
  50 parallel clients
  10 bytes payload

34686.09 requests per second
```

- 20字节
```Shell
====== SET ======
  100000 requests completed in 2.39 seconds
  50 parallel clients
  20 bytes payload

41893.59 requests per second

====== GET ======
  100000 requests completed in 2.28 seconds
  50 parallel clients
  20 bytes payload

43878.89 requests per second
```

- 50字节
```Shell
====== SET ======
  100000 requests completed in 2.47 seconds
  50 parallel clients
  50 bytes payload

40436.71 requests per second

====== GET ======
  100000 requests completed in 2.40 seconds
  50 parallel clients
  50 bytes payload

41614.64 requests per second
```

- 100字节
```Shell
====== SET ======
  100000 requests completed in 2.60 seconds
  50 parallel clients
  100 bytes payload

38446.75 requests per second

====== GET ======
  100000 requests completed in 2.35 seconds
  50 parallel clients
  100 bytes payload

42607.59 requests per second
```

- 200字节
```Shell
====== SET ======
  100000 requests completed in 2.52 seconds
  50 parallel clients
  200 bytes payload

39666.80 requests per second

====== GET ======
  100000 requests completed in 2.43 seconds
  50 parallel clients
  200 bytes payload

41084.63 requests per second
```

- 1k
```Shell
====== SET ======
  100000 requests completed in 2.67 seconds
  50 parallel clients
  1024 bytes payload

37439.16 requests per second

====== GET ======
  100000 requests completed in 2.45 seconds
  50 parallel clients
  1024 bytes payload
  
40816.32 requests per second
```

- 2k
```Shell
====== SET ======
  100000 requests completed in 2.67 seconds
  50 parallel clients
  2048 bytes payload
  
37411.15 requests per second

====== GET ======
  100000 requests completed in 2.56 seconds
  50 parallel clients
  2048 bytes payload

39047.25 requests per second
```
上边不同字节性能测试结果：
- 10字节的存储处理10万请求时间最长，每秒请求处理数量最低；
- 20字节的存储处理10万请求时间最短，每秒请求数量最多，效率最高；
- 50字节出存储处理10万请求时间略长于20字节，GET每秒请求数量比20字节少2000多，SET每秒请求数量比20字节少了1000多，
  效率明显降低了；
- 从100字节到2k越来越低，到1k和2k的时候差别不太大了
总结：20字节效率最高，其次是50字节，10字节效率最低。


#### 二、存入数据内存对比
- 存入前
```Shell
# Memory
used_memory:1081968
used_memory_human:1.03M
used_memory_rss:66023424
used_memory_rss_human:62.96M
used_memory_peak:63991008
used_memory_peak_human:61.03M
used_memory_peak_perc:1.69%
used_memory_overhead:1035252
used_memory_startup:1001280
used_memory_dataset:46716
used_memory_dataset_perc:57.90%
allocator_allocated:1045040
allocator_active:65985536
allocator_resident:65985536
total_system_memory:8589934592
total_system_memory_human:8.00G
used_memory_lua:37888
used_memory_lua_human:37.00K
used_memory_scripts:0
used_memory_scripts_human:0B
number_of_cached_scripts:0
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
allocator_frag_ratio:63.14
allocator_frag_bytes:64940496
allocator_rss_ratio:1.00
allocator_rss_bytes:0
rss_overhead_ratio:1.00
rss_overhead_bytes:37888
mem_fragmentation_ratio:63.18
mem_fragmentation_bytes:64978384
mem_not_counted_for_evict:0
mem_replication_backlog:0
mem_clients_slaves:0
mem_clients_normal:33972
mem_aof_buffer:0
mem_allocator:libc
active_defrag_running:0
lazyfree_pending_objects:0
```

- 50万条数据，每一个平均占用72.09字节
```Shell
# Memory
used_memory:37115536
used_memory_human:35.40M
used_memory_rss:87420928
used_memory_rss_human:83.37M
used_memory_peak:63991008
used_memory_peak_human:61.03M
used_memory_peak_perc:58.00%
used_memory_overhead:25229596
used_memory_startup:1001280
used_memory_dataset:11885940
used_memory_dataset_perc:32.91%
allocator_allocated:37078608
allocator_active:87383040
allocator_resident:87383040
total_system_memory:8589934592
total_system_memory_human:8.00G
used_memory_lua:37888
used_memory_lua_human:37.00K
used_memory_scripts:0
used_memory_scripts_human:0B
number_of_cached_scripts:0
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
allocator_frag_ratio:2.36
allocator_frag_bytes:50304432
allocator_rss_ratio:1.00
allocator_rss_bytes:0
rss_overhead_ratio:1.00
rss_overhead_bytes:37888
mem_fragmentation_ratio:2.36
mem_fragmentation_bytes:50342320
mem_not_counted_for_evict:0
mem_replication_backlog:0
mem_clients_slaves:0
mem_clients_normal:33972
mem_aof_buffer:0
mem_allocator:libc
active_defrag_running:0
lazyfree_pending_objects:0
```

- 20万数据，每个key平均占用73.71个字节
```Shell
# Memory
used_memory:15818800
used_memory_human:15.09M
used_memory_rss:66023424
used_memory_rss_human:62.96M
used_memory_peak:63991008
used_memory_peak_human:61.03M
used_memory_peak_perc:24.72%
used_memory_overhead:11132444
used_memory_startup:1001280
used_memory_dataset:4686356
used_memory_dataset_perc:31.63%
allocator_allocated:15781872
allocator_active:65985536
allocator_resident:65985536
total_system_memory:8589934592
total_system_memory_human:8.00G
used_memory_lua:37888
used_memory_lua_human:37.00K
used_memory_scripts:0
used_memory_scripts_human:0B
number_of_cached_scripts:0
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
allocator_frag_ratio:4.18
allocator_frag_bytes:50203664
allocator_rss_ratio:1.00
allocator_rss_bytes:0
rss_overhead_ratio:1.00
rss_overhead_bytes:37888
mem_fragmentation_ratio:4.18
mem_fragmentation_bytes:50241552
mem_not_counted_for_evict:0
mem_replication_backlog:0
mem_clients_slaves:0
mem_clients_normal:33972
mem_aof_buffer:0
mem_allocator:libc
active_defrag_running:0
lazyfree_pending_objects:0
```
- 10000条数据，每个key平均占用的内存为60.81个字节
```Shell
# Memory
used_memory:1692912
used_memory_human:1.61M
used_memory_rss:66023424
used_memory_rss_human:62.96M
used_memory_peak:63991008
used_memory_peak_human:61.03M
used_memory_peak_perc:2.65%
used_memory_overhead:1566364
used_memory_startup:1001280
used_memory_dataset:126548
used_memory_dataset_perc:18.30%
allocator_allocated:1655984
allocator_active:65985536
allocator_resident:65985536
total_system_memory:8589934592
total_system_memory_human:8.00G
used_memory_lua:37888
used_memory_lua_human:37.00K
used_memory_scripts:0
used_memory_scripts_human:0B
number_of_cached_scripts:0
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
allocator_frag_ratio:39.85
allocator_frag_bytes:64329552
allocator_rss_ratio:1.00
allocator_rss_bytes:0
rss_overhead_ratio:1.00
rss_overhead_bytes:37888
mem_fragmentation_ratio:39.87
mem_fragmentation_bytes:64367440
mem_not_counted_for_evict:0
mem_replication_backlog:0
mem_clients_slaves:0
mem_clients_normal:33972
mem_aof_buffer:0
mem_allocator:libc
active_defrag_running:0
lazyfree_pending_objects:0
```