# Metrics

|Name|description|type|
|--------------|-----------------|---------------|
|chrony_exporter_build_info|A metric with a constant '1' value labeled by version, revision, branch, goversion from which chrony_exporter was built, and the goos and goarch for the build. |gauge|
|chrony_tracking_frequency_ppms|Rate by which the system's clock would be wrong if chronyd was not correcting it, in PPMs |gauge|
|chrony_tracking_info|Chrony tracking info |gauge|
|chrony_tracking_last_offset_seconds|Chrony tracking last offset in seconds |gauge|
|chrony_tracking_reference_timestamp_seconds|Chrony tracking Reference timestamp |gauge|
|chrony_tracking_remote_reference|Chrony tracking is connected to a remote source |gauge|
|chrony_tracking_residual_frequency_ppms|For the currently selected reference source, the difference between the frequency it suggests and the one currently in use, in PPMs |gauge|
|chrony_tracking_rms_offset_seconds|Chrony tracking long-term average of the offset |gauge|
|chrony_tracking_root_delay_seconds|This is the total of the network path delays to the stratum-1 computer from which the computer is ultimately synchronised |gauge|
|chrony_tracking_root_dispersion_seconds|Chrony tracking total of all measurement errors to the NTP root |gauge|
|chrony_tracking_skew_ppms|The estimated error bound on the frequency, in PPMs |gauge|
|chrony_tracking_stratum|Chrony tracking client stratum |gauge|
|chrony_tracking_system_time_seconds|Chrony tracking System time |gauge|
|chrony_tracking_update_interval_seconds|The time elapsed since the last measurement from the reference source was processed, in seconds |gauge|
|chrony_up|Whether the chrony server is up. |gauge|
|go_gc_duration_seconds|A summary of the pause duration of garbage collection cycles. |summary|
|go_goroutines|Number of goroutines that currently exist. |gauge|
|go_info|Information about the Go environment. |gauge|
|go_memstats_alloc_bytes|Number of bytes allocated and still in use. |gauge|
|go_memstats_alloc_bytes_total|Total number of bytes allocated, even if freed. |counter|
|go_memstats_buck_hash_sys_bytes|Number of bytes used by the profiling bucket hash table. |gauge|
|go_memstats_frees_total|Total number of frees. |counter|
|go_memstats_gc_sys_bytes|Number of bytes used for garbage collection system metadata. |gauge|
|go_memstats_heap_alloc_bytes|Number of heap bytes allocated and still in use. |gauge|
|go_memstats_heap_idle_bytes|Number of heap bytes waiting to be used. |gauge|
|go_memstats_heap_inuse_bytes|Number of heap bytes that are in use. |gauge|
|go_memstats_heap_objects|Number of allocated objects. |gauge|
|go_memstats_heap_released_bytes|Number of heap bytes released to OS. |gauge|
|go_memstats_heap_sys_bytes|Number of heap bytes obtained from system. |gauge|
|go_memstats_last_gc_time_seconds|Number of seconds since 1970 of last garbage collection. |gauge|
|go_memstats_lookups_total|Total number of pointer lookups. |counter|
|go_memstats_mallocs_total|Total number of mallocs. |counter|
|go_memstats_mcache_inuse_bytes|Number of bytes in use by mcache structures. |gauge|
|go_memstats_mcache_sys_bytes|Number of bytes used for mcache structures obtained from system. |gauge|
|go_memstats_mspan_inuse_bytes|Number of bytes in use by mspan structures. |gauge|
|go_memstats_mspan_sys_bytes|Number of bytes used for mspan structures obtained from system. |gauge|
|go_memstats_next_gc_bytes|Number of heap bytes when next garbage collection will take place. |gauge|
|go_memstats_other_sys_bytes|Number of bytes used for other system allocations. |gauge|
|go_memstats_stack_inuse_bytes|Number of bytes in use by the stack allocator. |gauge|
|go_memstats_stack_sys_bytes|Number of bytes obtained from system for stack allocator. |gauge|
|go_memstats_sys_bytes|Number of bytes obtained from system. |gauge|
|go_threads|Number of OS threads created. |gauge|
|process_cpu_seconds_total|Total user and system CPU time spent in seconds. |counter|
|process_max_fds|Maximum number of open file descriptors. |gauge|
|process_open_fds|Number of open file descriptors. |gauge|
|process_resident_memory_bytes|Resident memory size in bytes. |gauge|
|process_start_time_seconds|Start time of the process since unix epoch in seconds. |gauge|
|process_virtual_memory_bytes|Virtual memory size in bytes. |gauge|
|process_virtual_memory_max_bytes|Maximum amount of virtual memory available in bytes. |gauge|
|promhttp_metric_handler_requests_in_flight|Current number of scrapes being served. |gauge|
|promhttp_metric_handler_requests_total|Total number of scrapes by HTTP status code. |counter|
