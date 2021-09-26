# Mysql

## 安装
```
# 启动mysql 
sudo systemctl start mysql
# 查看mysql 状态
sudo systemctl status mysql

# 查看mysql 配置 
vim /etc/mysql/mariadb.conf.d/50-server.cnf

# 查看系统中的mysql进程
ps -aux | grep "mysql"
ps -ef | grep mysql

# 杀掉mysql进程 提示 bash: kill: (4385) - Operation not permitted， 因为没有权限，要使用sudo
sudo kill -15 
```
- 问题：访问数据库报错 ERROR 1524 (HY000): Plugin 'unix_socket' is not loaded

  解决：在配置文件`[mysqld]` 下增加 `plugin-load-add = auth_socket.so`， 重启服务。

## 安全管理
### 管理用户
- 查看用户
    ```
    USE mysql;
    SELECT user FROM user;
    ```
- 创建用户 spaceack, 密码为 password123
    ```
    CREATE USER spaceack IDENTIFIED BY 'password123';
    ```
- 为用户授予数据库权限
  ```
  grant all privileges on `数据库名`.* to '用户名'@'localhost';
  flush privileges;
  ```
## 命令
连接数据库管理系统
mysql -u root -p -h localhost -P 3306

查看所有数据库
SHOW DATABASES;

使用数据库

USE mysql;

查看数据库中的所有表列表

SHOW TABLES;

显示表列

SHOW COLUMNS FROM help_keyword;
```
+-----------------+------------------+------+-----+---------+-------+
| Field           | Type             | Null | Key | Default | Extra |
+-----------------+------------------+------+-----+---------+-------+
| help_keyword_id | int(10) unsigned | NO   | PRI | NULL    |       |
| name            | char(64)         | NO   | UNI | NULL    |       |
+-----------------+------------------+------+-----+---------+-------+

DESCRIBE help_keyword;
DESC help_keyword;

显示建库语句
 SHOW CREATE DATABASE mysql;
显示建表语句
 SHOW CREATE TABLE help_keyword;

显示用户安全权限
SHOW GRANTS

```
显示服务器状态：
SHOW STATUS;
```
+--------------------------------------------------------+--------------------------------------------------+
| Variable_name                                          | Value                                            |
+--------------------------------------------------------+--------------------------------------------------+
| Aborted_clients                                        | 46                                               |
| Aborted_connects                                       | 1                                                |
| Aborted_connects_preauth                               | 0                                                |
| Access_denied_errors                                   | 0                                                |
| Acl_column_grants                                      | 0                                                |
| Acl_database_grants                                    | 1                                                |
| Acl_function_grants                                    | 0                                                |
| Acl_procedure_grants                                   | 0                                                |
| Acl_package_spec_grants                                | 0                                                |
| Acl_package_body_grants                                | 0                                                |
| Acl_proxy_users                                        | 1                                                |
| Acl_role_grants                                        | 0                                                |
| Acl_roles                                              | 0                                                |
| Acl_table_grants                                       | 2                                                |
| Acl_users                                              | 4                                                |
| Aria_pagecache_blocks_not_flushed                      | 0                                                |
| Aria_pagecache_blocks_unused                           | 15634                                            |
| Aria_pagecache_blocks_used                             | 16                                               |
| Aria_pagecache_read_requests                           | 8513                                             |
| Aria_pagecache_reads                                   | 671                                              |
| Aria_pagecache_write_requests                          | 1306                                             |
| Aria_pagecache_writes                                  | 1306                                             |
| Aria_transaction_log_syncs                             | 1                                                |
| Binlog_commits                                         | 0                                                |
| Binlog_group_commits                                   | 0                                                |
| Binlog_group_commit_trigger_count                      | 0                                                |
| Binlog_group_commit_trigger_lock_wait                  | 0                                                |
| Binlog_group_commit_trigger_timeout                    | 0                                                |
| Binlog_snapshot_file                                   |                                                  |
| Binlog_snapshot_position                               | 0                                                |
| Binlog_bytes_written                                   | 0                                                |
| Binlog_cache_disk_use                                  | 0                                                |
| Binlog_cache_use                                       | 0                                                |
| Binlog_stmt_cache_disk_use                             | 0                                                |
| Binlog_stmt_cache_use                                  | 0                                                |
| Busy_time                                              | 0.000000                                         |
| Bytes_received                                         | 1118                                             |
| Bytes_sent                                             | 28819                                            |
| Column_compressions                                    | 0                                                |
| Column_decompressions                                  | 0                                                |
| Com_admin_commands                                     | 0                                                |
| Com_alter_db                                           | 0                                                |
| Com_alter_db_upgrade                                   | 0                                                |
| Com_alter_event                                        | 0                                                |
| Com_alter_function                                     | 0                                                |
| Com_alter_procedure                                    | 0                                                |
| Com_alter_server                                       | 0                                                |
| Com_alter_sequence                                     | 0                                                |
| Com_alter_table                                        | 0                                                |
| Com_alter_tablespace                                   | 0                                                |
| Com_alter_user                                         | 0                                                |
| Com_analyze                                            | 0                                                |
| Com_assign_to_keycache                                 | 0                                                |
| Com_backup                                             | 0                                                |
| Com_backup_lock                                        | 0                                                |
| Com_begin                                              | 0                                                |
| Com_binlog                                             | 0                                                |
| Com_call_procedure                                     | 0                                                |
| Com_change_db                                          | 1                                                |
| Com_change_master                                      | 0                                                |
| Com_check                                              | 0                                                |
| Com_checksum                                           | 0                                                |
| Com_commit                                             | 0                                                |
| Com_compound_sql                                       | 0                                                |
| Com_create_db                                          | 0                                                |
| Com_create_event                                       | 0                                                |
| Com_create_function                                    | 0                                                |
| Com_create_index                                       | 0                                                |
| Com_create_package                                     | 0                                                |
| Com_create_package_body                                | 0                                                |
| Com_create_procedure                                   | 0                                                |
| Com_create_role                                        | 0                                                |
| Com_create_sequence                                    | 0                                                |
| Com_create_server                                      | 0                                                |
| Com_create_table                                       | 0                                                |
| Com_create_temporary_table                             | 0                                                |
| Com_create_trigger                                     | 0                                                |
| Com_create_udf                                         | 0                                                |
| Com_create_user                                        | 0                                                |
| Com_create_view                                        | 0                                                |
| Com_dealloc_sql                                        | 0                                                |
| Com_delete                                             | 0                                                |
| Com_delete_multi                                       | 0                                                |
| Com_do                                                 | 0                                                |
| Com_drop_db                                            | 0                                                |
| Com_drop_event                                         | 0                                                |
| Com_drop_function                                      | 0                                                |
| Com_drop_index                                         | 0                                                |
| Com_drop_procedure                                     | 0                                                |
| Com_drop_package                                       | 0                                                |
| Com_drop_package_body                                  | 0                                                |
| Com_drop_role                                          | 0                                                |
| Com_drop_server                                        | 0                                                |
| Com_drop_sequence                                      | 0                                                |
| Com_drop_table                                         | 0                                                |
| Com_drop_temporary_table                               | 0                                                |
| Com_drop_trigger                                       | 0                                                |
| Com_drop_user                                          | 0                                                |
| Com_drop_view                                          | 0                                                |
| Com_empty_query                                        | 0                                                |
| Com_execute_immediate                                  | 0                                                |
| Com_execute_sql                                        | 0                                                |
| Com_flush                                              | 0                                                |
| Com_get_diagnostics                                    | 0                                                |
| Com_grant                                              | 0                                                |
| Com_grant_role                                         | 0                                                |
| Com_ha_close                                           | 0                                                |
| Com_ha_open                                            | 0                                                |
| Com_ha_read                                            | 0                                                |
| Com_help                                               | 0                                                |
| Com_insert                                             | 0                                                |
| Com_insert_select                                      | 0                                                |
| Com_install_plugin                                     | 0                                                |
| Com_kill                                               | 0                                                |
| Com_load                                               | 0                                                |
| Com_lock_tables                                        | 0                                                |
| Com_multi                                              | 0                                                |
| Com_optimize                                           | 0                                                |
| Com_preload_keys                                       | 0                                                |
| Com_prepare_sql                                        | 0                                                |
| Com_purge                                              | 0                                                |
| Com_purge_before_date                                  | 0                                                |
| Com_release_savepoint                                  | 0                                                |
| Com_rename_table                                       | 0                                                |
| Com_rename_user                                        | 0                                                |
| Com_repair                                             | 0                                                |
| Com_replace                                            | 0                                                |
| Com_replace_select                                     | 0                                                |
| Com_reset                                              | 0                                                |
| Com_resignal                                           | 0                                                |
| Com_revoke                                             | 0                                                |
| Com_revoke_all                                         | 0                                                |
| Com_revoke_role                                        | 0                                                |
| Com_rollback                                           | 0                                                |
| Com_rollback_to_savepoint                              | 0                                                |
| Com_savepoint                                          | 0                                                |
| Com_select                                             | 2                                                |
| Com_set_option                                         | 0                                                |
| Com_show_authors                                       | 0                                                |
| Com_show_binlog_events                                 | 0                                                |
| Com_show_binlogs                                       | 0                                                |
| Com_show_charsets                                      | 0                                                |
| Com_show_collations                                    | 0                                                |
| Com_show_contributors                                  | 0                                                |
| Com_show_create_db                                     | 0                                                |
| Com_show_create_event                                  | 0                                                |
| Com_show_create_func                                   | 0                                                |
| Com_show_create_package                                | 0                                                |
| Com_show_create_package_body                           | 0                                                |
| Com_show_create_proc                                   | 0                                                |
| Com_show_create_table                                  | 0                                                |
| Com_show_create_trigger                                | 0                                                |
| Com_show_create_user                                   | 0                                                |
| Com_show_databases                                     | 1                                                |
| Com_show_engine_logs                                   | 0                                                |
| Com_show_engine_mutex                                  | 0                                                |
| Com_show_engine_status                                 | 0                                                |
| Com_show_errors                                        | 0                                                |
| Com_show_events                                        | 0                                                |
| Com_show_explain                                       | 0                                                |
| Com_show_fields                                        | 37                                               |
| Com_show_function_status                               | 0                                                |
| Com_show_generic                                       | 0                                                |
| Com_show_grants                                        | 0                                                |
| Com_show_keys                                          | 0                                                |
| Com_show_binlog_status                                 | 0                                                |
| Com_show_open_tables                                   | 0                                                |
| Com_show_package_status                                | 0                                                |
| Com_show_package_body_status                           | 0                                                |
| Com_show_plugins                                       | 0                                                |
| Com_show_privileges                                    | 0                                                |
| Com_show_procedure_status                              | 0                                                |
| Com_show_processlist                                   | 0                                                |
| Com_show_profile                                       | 0                                                |
| Com_show_profiles                                      | 0                                                |
| Com_show_relaylog_events                               | 0                                                |
| Com_show_slave_hosts                                   | 0                                                |
| Com_show_slave_status                                  | 0                                                |
| Com_show_status                                        | 1                                                |
| Com_show_storage_engines                               | 0                                                |
| Com_show_table_status                                  | 0                                                |
| Com_show_tables                                        | 3                                                |
| Com_show_triggers                                      | 0                                                |
| Com_show_variables                                     | 0                                                |
| Com_show_warnings                                      | 0                                                |
| Com_shutdown                                           | 0                                                |
| Com_signal                                             | 0                                                |
| Com_start_all_slaves                                   | 0                                                |
| Com_start_slave                                        | 0                                                |
| Com_stmt_close                                         | 0                                                |
| Com_stmt_execute                                       | 0                                                |
| Com_stmt_fetch                                         | 0                                                |
| Com_stmt_prepare                                       | 0                                                |
| Com_stmt_reprepare                                     | 0                                                |
| Com_stmt_reset                                         | 0                                                |
| Com_stmt_send_long_data                                | 0                                                |
| Com_stop_all_slaves                                    | 0                                                |
| Com_stop_slave                                         | 0                                                |
| Com_truncate                                           | 0                                                |
| Com_uninstall_plugin                                   | 0                                                |
| Com_unlock_tables                                      | 0                                                |
| Com_update                                             | 0                                                |
| Com_update_multi                                       | 0                                                |
| Com_xa_commit                                          | 0                                                |
| Com_xa_end                                             | 0                                                |
| Com_xa_prepare                                         | 0                                                |
| Com_xa_recover                                         | 0                                                |
| Com_xa_rollback                                        | 0                                                |
| Com_xa_start                                           | 0                                                |
| Compression                                            | OFF                                              |
| Connection_errors_accept                               | 0                                                |
| Connection_errors_internal                             | 0                                                |
| Connection_errors_max_connections                      | 0                                                |
| Connection_errors_peer_address                         | 0                                                |
| Connection_errors_select                               | 0                                                |
| Connection_errors_tcpwrap                              | 0                                                |
| Connections                                            | 87                                               |
| Cpu_time                                               | 0.000000                                         |
| Created_tmp_disk_tables                                | 6                                                |
| Created_tmp_files                                      | 5                                                |
| Created_tmp_tables                                     | 10                                               |
| Delayed_errors                                         | 0                                                |
| Delayed_insert_threads                                 | 0                                                |
| Delayed_writes                                         | 0                                                |
| Delete_scan                                            | 0                                                |
| Empty_queries                                          | 0                                                |
| Executed_events                                        | 0                                                |
| Executed_triggers                                      | 0                                                |
| Feature_application_time_periods                       | 0                                                |
| Feature_check_constraint                               | 85                                               |
| Feature_custom_aggregate_functions                     | 0                                                |
| Feature_delay_key_write                                | 0                                                |
| Feature_dynamic_columns                                | 0                                                |
| Feature_fulltext                                       | 0                                                |
| Feature_gis                                            | 0                                                |
| Feature_insert_returning                               | 0                                                |
| Feature_invisible_columns                              | 0                                                |
| Feature_json                                           | 90                                               |
| Feature_locale                                         | 0                                                |
| Feature_subquery                                       | 0                                                |
| Feature_system_versioning                              | 0                                                |
| Feature_timezone                                       | 0                                                |
| Feature_trigger                                        | 0                                                |
| Feature_window_functions                               | 0                                                |
| Feature_xml                                            | 0                                                |
| Handler_commit                                         | 0                                                |
| Handler_delete                                         | 0                                                |
| Handler_discover                                       | 0                                                |
| Handler_external_lock                                  | 0                                                |
| Handler_icp_attempts                                   | 0                                                |
| Handler_icp_match                                      | 0                                                |
| Handler_mrr_init                                       | 0                                                |
| Handler_mrr_key_refills                                | 0                                                |
| Handler_mrr_rowid_refills                              | 0                                                |
| Handler_prepare                                        | 0                                                |
| Handler_read_first                                     | 0                                                |
| Handler_read_key                                       | 0                                                |
| Handler_read_last                                      | 0                                                |
| Handler_read_next                                      | 0                                                |
| Handler_read_prev                                      | 0                                                |
| Handler_read_retry                                     | 0                                                |
| Handler_read_rnd                                       | 0                                                |
| Handler_read_rnd_deleted                               | 0                                                |
| Handler_read_rnd_next                                  | 207                                              |
| Handler_rollback                                       | 0                                                |
| Handler_savepoint                                      | 0                                                |
| Handler_savepoint_rollback                             | 0                                                |
| Handler_tmp_delete                                     | 0                                                |
| Handler_tmp_update                                     | 0                                                |
| Handler_tmp_write                                      | 197                                              |
| Handler_update                                         | 0                                                |
| Handler_write                                          | 0                                                |
| Innodb_adaptive_hash_hash_searches                     | 0                                                |
| Innodb_adaptive_hash_non_hash_searches                 | 27630                                            |
| Innodb_background_log_sync                             | 39202                                            |
| Innodb_buffered_aio_submitted                          | 1                                                |
| Innodb_buffer_pool_dump_status                         |                                                  |
| Innodb_buffer_pool_load_status                         | Buffer pool(s) load completed at 200807  8:53:47 |
| Innodb_buffer_pool_resize_status                       |                                                  |
| Innodb_buffer_pool_load_incomplete                     | OFF                                              |
| Innodb_buffer_pool_pages_data                          | 857                                              |
| Innodb_buffer_pool_bytes_data                          | 14041088                                         |
| Innodb_buffer_pool_pages_dirty                         | 0                                                |
| Innodb_buffer_pool_bytes_dirty                         | 0                                                |
| Innodb_buffer_pool_pages_flushed                       | 2540                                             |
| Innodb_buffer_pool_pages_free                          | 7197                                             |
| Innodb_buffer_pool_pages_made_not_young                | 6                                                |
| Innodb_buffer_pool_pages_made_young                    | 277                                              |
| Innodb_buffer_pool_pages_misc                          | 0                                                |
| Innodb_buffer_pool_pages_old                           | 296                                              |
| Innodb_buffer_pool_pages_total                         | 8054                                             |
| Innodb_buffer_pool_pages_lru_flushed                   | 0                                                |
| Innodb_buffer_pool_read_ahead_rnd                      | 0                                                |
| Innodb_buffer_pool_read_ahead                          | 0                                                |
| Innodb_buffer_pool_read_ahead_evicted                  | 0                                                |
| Innodb_buffer_pool_read_requests                       | 64404                                            |
| Innodb_buffer_pool_reads                               | 509                                              |
| Innodb_buffer_pool_wait_free                           | 0                                                |
| Innodb_buffer_pool_write_requests                      | 17824                                            |
| Innodb_checkpoint_age                                  | 12                                               |
| Innodb_checkpoint_max_age                              | 80827823                                         |
| Innodb_data_fsyncs                                     | 2195                                             |
| Innodb_data_pending_fsyncs                             | 0                                                |
| Innodb_data_pending_reads                              | 0                                                |
| Innodb_data_pending_writes                             | 0                                                |
| Innodb_data_read                                       | 8339456                                          |
| Innodb_data_reads                                      | 556                                              |
| Innodb_data_writes                                     | 4335                                             |
| Innodb_data_written                                    | 76726272                                         |
| Innodb_dblwr_pages_written                             | 2155                                             |
| Innodb_dblwr_writes                                    | 105                                              |
| Innodb_deadlocks                                       | 0                                                |
| Innodb_history_list_length                             | 5                                                |
| Innodb_ibuf_discarded_delete_marks                     | 0                                                |
| Innodb_ibuf_discarded_deletes                          | 0                                                |
| Innodb_ibuf_discarded_inserts                          | 0                                                |
| Innodb_ibuf_free_list                                  | 0                                                |
| Innodb_ibuf_merged_delete_marks                        | 0                                                |
| Innodb_ibuf_merged_deletes                             | 0                                                |
| Innodb_ibuf_merged_inserts                             | 0                                                |
| Innodb_ibuf_merges                                     | 0                                                |
| Innodb_ibuf_segment_size                               | 2                                                |
| Innodb_ibuf_size                                       | 1                                                |
| Innodb_log_waits                                       | 0                                                |
| Innodb_log_write_requests                              | 1742                                             |
| Innodb_log_writes                                      | 1639                                             |
| Innodb_lsn_current                                     | 15076710                                         |
| Innodb_lsn_flushed                                     | 15076710                                         |
| Innodb_lsn_last_checkpoint                             | 15076698                                         |
| Innodb_master_thread_active_loops                      | 193                                              |
| Innodb_master_thread_idle_loops                        | 39012                                            |
| Innodb_max_trx_id                                      | 15931                                            |
| Innodb_mem_adaptive_hash                               | 0                                                |
| Innodb_mem_dictionary                                  | 947048                                           |
| Innodb_os_log_fsyncs                                   | 1639                                             |
| Innodb_os_log_pending_fsyncs                           | 0                                                |
| Innodb_os_log_pending_writes                           | 0                                                |
| Innodb_os_log_written                                  | 2279424                                          |
| Innodb_page_size                                       | 16384                                            |
| Innodb_pages_created                                   | 360                                              |
| Innodb_pages_read                                      | 509                                              |
| Innodb_pages_written                                   | 2528                                             |
| Innodb_row_lock_current_waits                          | 0                                                |
| Innodb_row_lock_time                                   | 0                                                |
| Innodb_row_lock_time_avg                               | 0                                                |
| Innodb_row_lock_time_max                               | 0                                                |
| Innodb_row_lock_waits                                  | 0                                                |
| Innodb_rows_deleted                                    | 4                                                |
| Innodb_rows_inserted                                   | 687                                              |
| Innodb_rows_read                                       | 7097                                             |
| Innodb_rows_updated                                    | 46                                               |
| Innodb_system_rows_deleted                             | 0                                                |
| Innodb_system_rows_inserted                            | 0                                                |
| Innodb_system_rows_read                                | 0                                                |
| Innodb_system_rows_updated                             | 0                                                |
| Innodb_num_open_files                                  | 37                                               |
| Innodb_truncated_status_writes                         | 0                                                |
| Innodb_available_undo_logs                             | 128                                              |
| Innodb_undo_truncations                                | 0                                                |
| Innodb_page_compression_saved                          | 0                                                |
| Innodb_num_index_pages_written                         | 0                                                |
| Innodb_num_non_index_pages_written                     | 0                                                |
| Innodb_num_pages_page_compressed                       | 0                                                |
| Innodb_num_page_compressed_trim_op                     | 0                                                |
| Innodb_num_pages_page_decompressed                     | 0                                                |
| Innodb_num_pages_page_compression_error                | 0                                                |
| Innodb_num_pages_encrypted                             | 0                                                |
| Innodb_num_pages_decrypted                             | 0                                                |
| Innodb_have_lz4                                        | ON                                               |
| Innodb_have_lzo                                        | OFF                                              |
| Innodb_have_lzma                                       | OFF                                              |
| Innodb_have_bzip2                                      | OFF                                              |
| Innodb_have_snappy                                     | OFF                                              |
| Innodb_have_punch_hole                                 | ON                                               |
| Innodb_defragment_compression_failures                 | 0                                                |
| Innodb_defragment_failures                             | 0                                                |
| Innodb_defragment_count                                | 0                                                |
| Innodb_instant_alter_column                            | 0                                                |
| Innodb_onlineddl_rowlog_rows                           | 0                                                |
| Innodb_onlineddl_rowlog_pct_used                       | 0                                                |
| Innodb_onlineddl_pct_progress                          | 0                                                |
| Innodb_secondary_index_triggered_cluster_reads         | 4065                                             |
| Innodb_secondary_index_triggered_cluster_reads_avoided | 0                                                |
| Innodb_encryption_rotation_pages_read_from_cache       | 0                                                |
| Innodb_encryption_rotation_pages_read_from_disk        | 0                                                |
| Innodb_encryption_rotation_pages_modified              | 0                                                |
| Innodb_encryption_rotation_pages_flushed               | 0                                                |
| Innodb_encryption_rotation_estimated_iops              | 0                                                |
| Innodb_encryption_key_rotation_list_length             | 0                                                |
| Innodb_encryption_n_merge_blocks_encrypted             | 0                                                |
| Innodb_encryption_n_merge_blocks_decrypted             | 0                                                |
| Innodb_encryption_n_rowlog_blocks_encrypted            | 0                                                |
| Innodb_encryption_n_rowlog_blocks_decrypted            | 0                                                |
| Innodb_encryption_n_temp_blocks_encrypted              | 0                                                |
| Innodb_encryption_n_temp_blocks_decrypted              | 0                                                |
| Innodb_encryption_num_key_requests                     | 0                                                |
| Key_blocks_not_flushed                                 | 0                                                |
| Key_blocks_unused                                      | 107163                                           |
| Key_blocks_used                                        | 0                                                |
| Key_blocks_warm                                        | 0                                                |
| Key_read_requests                                      | 0                                                |
| Key_reads                                              | 0                                                |
| Key_write_requests                                     | 0                                                |
| Key_writes                                             | 0                                                |
| Last_query_cost                                        | 2.399000                                         |
| Master_gtid_wait_count                                 | 0                                                |
| Master_gtid_wait_time                                  | 0                                                |
| Master_gtid_wait_timeouts                              | 0                                                |
| Max_statement_time_exceeded                            | 0                                                |
| Max_used_connections                                   | 5                                                |
| Memory_used                                            | 72528                                            |
| Memory_used_initial                                    | 34167904                                         |
| Not_flushed_delayed_rows                               | 0                                                |
| Open_files                                             | 56                                               |
| Open_streams                                           | 4                                                |
| Open_table_definitions                                 | 142                                              |
| Open_tables                                            | 142                                              |
| Opened_files                                           | 4257                                             |
| Opened_plugin_libraries                                | 0                                                |
| Opened_table_definitions                               | 0                                                |
| Opened_tables                                          | 0                                                |
| Opened_views                                           | 2                                                |
| Performance_schema_accounts_lost                       | 0                                                |
| Performance_schema_cond_classes_lost                   | 0                                                |
| Performance_schema_cond_instances_lost                 | 0                                                |
| Performance_schema_digest_lost                         | 0                                                |
| Performance_schema_file_classes_lost                   | 0                                                |
| Performance_schema_file_handles_lost                   | 0                                                |
| Performance_schema_file_instances_lost                 | 0                                                |
| Performance_schema_hosts_lost                          | 0                                                |
| Performance_schema_index_stat_lost                     | 0                                                |
| Performance_schema_locker_lost                         | 0                                                |
| Performance_schema_memory_classes_lost                 | 0                                                |
| Performance_schema_metadata_lock_lost                  | 0                                                |
| Performance_schema_mutex_classes_lost                  | 0                                                |
| Performance_schema_mutex_instances_lost                | 0                                                |
| Performance_schema_nested_statement_lost               | 0                                                |
| Performance_schema_prepared_statements_lost            | 0                                                |
| Performance_schema_program_lost                        | 0                                                |
| Performance_schema_rwlock_classes_lost                 | 0                                                |
| Performance_schema_rwlock_instances_lost               | 0                                                |
| Performance_schema_session_connect_attrs_lost          | 0                                                |
| Performance_schema_socket_classes_lost                 | 0                                                |
| Performance_schema_socket_instances_lost               | 0                                                |
| Performance_schema_stage_classes_lost                  | 0                                                |
| Performance_schema_statement_classes_lost              | 0                                                |
| Performance_schema_table_handles_lost                  | 0                                                |
| Performance_schema_table_instances_lost                | 0                                                |
| Performance_schema_table_lock_stat_lost                | 0                                                |
| Performance_schema_thread_classes_lost                 | 0                                                |
| Performance_schema_thread_instances_lost               | 0                                                |
| Performance_schema_users_lost                          | 0                                                |
| Prepared_stmt_count                                    | 0                                                |
| Qcache_free_blocks                                     | 1                                                |
| Qcache_free_memory                                     | 1031304                                          |
| Qcache_hits                                            | 0                                                |
| Qcache_inserts                                         | 0                                                |
| Qcache_lowmem_prunes                                   | 0                                                |
| Qcache_not_cached                                      | 0                                                |
| Qcache_queries_in_cache                                | 0                                                |
| Qcache_total_blocks                                    | 1                                                |
| Queries                                                | 10175                                            |
| Questions                                              | 48                                               |
| Rows_read                                              | 0                                                |
| Rows_sent                                              | 199                                              |
| Rows_tmp_read                                          | 197                                              |
| Rpl_semi_sync_master_clients                           | 0                                                |
| Rpl_semi_sync_master_get_ack                           | 0                                                |
| Rpl_semi_sync_master_net_avg_wait_time                 | 0                                                |
| Rpl_semi_sync_master_net_wait_time                     | 0                                                |
| Rpl_semi_sync_master_net_waits                         | 0                                                |
| Rpl_semi_sync_master_no_times                          | 0                                                |
| Rpl_semi_sync_master_no_tx                             | 0                                                |
| Rpl_semi_sync_master_request_ack                       | 0                                                |
| Rpl_semi_sync_master_status                            | OFF                                              |
| Rpl_semi_sync_master_timefunc_failures                 | 0                                                |
| Rpl_semi_sync_master_tx_avg_wait_time                  | 0                                                |
| Rpl_semi_sync_master_tx_wait_time                      | 0                                                |
| Rpl_semi_sync_master_tx_waits                          | 0                                                |
| Rpl_semi_sync_master_wait_pos_backtraverse             | 0                                                |
| Rpl_semi_sync_master_wait_sessions                     | 0                                                |
| Rpl_semi_sync_master_yes_tx                            | 0                                                |
| Rpl_semi_sync_slave_send_ack                           | 0                                                |
| Rpl_semi_sync_slave_status                             | OFF                                              |
| Rpl_status                                             | AUTH_MASTER                                      |
| Rpl_transactions_multi_engine                          | 0                                                |
| Select_full_join                                       | 0                                                |
| Select_full_range_join                                 | 0                                                |
| Select_range                                           | 0                                                |
| Select_range_check                                     | 0                                                |
| Select_scan                                            | 10                                               |
| Slave_connections                                      | 0                                                |
| Slave_heartbeat_period                                 | 0.000                                            |
| Slave_open_temp_tables                                 | 0                                                |
| Slave_received_heartbeats                              | 0                                                |
| Slave_retried_transactions                             | 0                                                |
| Slave_running                                          | OFF                                              |
| Slave_skipped_errors                                   | 0                                                |
| Slaves_connected                                       | 0                                                |
| Slaves_running                                         | 0                                                |
| Slow_launch_threads                                    | 0                                                |
| Slow_queries                                           | 0                                                |
| Sort_merge_passes                                      | 0                                                |
| Sort_priority_queue_sorts                              | 0                                                |
| Sort_range                                             | 0                                                |
| Sort_rows                                              | 0                                                |
| Sort_scan                                              | 0                                                |
| Ssl_accept_renegotiates                                | 0                                                |
| Ssl_accepts                                            | 0                                                |
| Ssl_callback_cache_hits                                | 0                                                |
| Ssl_cipher                                             |                                                  |
| Ssl_cipher_list                                        |                                                  |
| Ssl_client_connects                                    | 0                                                |
| Ssl_connect_renegotiates                               | 0                                                |
| Ssl_ctx_verify_depth                                   | 0                                                |
| Ssl_ctx_verify_mode                                    | 0                                                |
| Ssl_default_timeout                                    | 0                                                |
| Ssl_finished_accepts                                   | 0                                                |
| Ssl_finished_connects                                  | 0                                                |
| Ssl_server_not_after                                   |                                                  |
| Ssl_server_not_before                                  |                                                  |
| Ssl_session_cache_hits                                 | 0                                                |
| Ssl_session_cache_misses                               | 0                                                |
| Ssl_session_cache_mode                                 | NONE                                             |
| Ssl_session_cache_overflows                            | 0                                                |
| Ssl_session_cache_size                                 | 0                                                |
| Ssl_session_cache_timeouts                             | 0                                                |
| Ssl_sessions_reused                                    | 0                                                |
| Ssl_used_session_cache_entries                         | 0                                                |
| Ssl_verify_depth                                       | 0                                                |
| Ssl_verify_mode                                        | 0                                                |
| Ssl_version                                            |                                                  |
| Subquery_cache_hit                                     | 0                                                |
| Subquery_cache_miss                                    | 0                                                |
| Syncs                                                  | 145                                              |
| Table_locks_immediate                                  | 372                                              |
| Table_locks_waited                                     | 0                                                |
| Table_open_cache_active_instances                      | 1                                                |
| Table_open_cache_hits                                  | 37                                               |
| Table_open_cache_misses                                | 2                                                |
| Table_open_cache_overflows                             | 0                                                |
| Tc_log_max_pages_used                                  | 0                                                |
| Tc_log_page_size                                       | 0                                                |
| Tc_log_page_waits                                      | 0                                                |
| Threadpool_idle_threads                                | 0                                                |
| Threadpool_threads                                     | 0                                                |
| Threads_cached                                         | 0                                                |
| Threads_connected                                      | 3                                                |
| Threads_created                                        | 15                                               |
| Threads_running                                        | 1                                                |
| Transactions_gtid_foreign_engine                       | 0                                                |
| Transactions_multi_engine                              | 0                                                |
| Update_scan                                            | 0                                                |
| Uptime                                                 | 88125                                            |
| Uptime_since_flush_status                              | 88125                                            |
| wsrep                                                  | 228475353144                                     |
| wsrep_applier_thread_count                             | 0                                                |
| wsrep_cluster_capabilities                             |                                                  |
| wsrep_cluster_conf_id                                  | 18446744073709551615                             |
| wsrep_cluster_size                                     | 0                                                |
| wsrep_cluster_state_uuid                               |                                                  |
| wsrep_cluster_status                                   | Disconnected                                     |
| wsrep_connected                                        | OFF                                              |
| wsrep_local_bf_aborts                                  | 0                                                |
| wsrep_local_index                                      | 18446744073709551615                             |
| wsrep_provider_capabilities                            |                                                  |
| wsrep_provider_name                                    |                                                  |
| wsrep_provider_vendor                                  |                                                  |
| wsrep_provider_version                                 |                                                  |
| wsrep_ready                                            | OFF                                              |
| wsrep_rollbacker_thread_count                          | 0                                                |
| wsrep_thread_count                                     | 0                                                |
+--------------------------------------------------------+--------------------------------------------------+

```

## 备份
### Mysql备份类型
#### 按照备份时对数据库的影响分为
 - Hot backup（热备）：也叫在线备份。指在数据库运行中直接备份，对正在运行的数据库没有任何影响。
 - Cold backup（冷备）：也叫离线备份。指在数据库停止的情况下备份。
 - Warm backup（温备）：在数据库运行时备份，会加一个全局锁以保证数据的一致性，会对当前数据库的操作有影响。
#### 按照备份后的文件内容分为
 - 逻辑备份：指备份后的文件内容是可读的，通常为文本文件，内容一般是SQL语句或表内的实际数据（mysqldump和select * into outfile），一般适用于数据库的升级和迁移，还原时间较长。适用于中小型数据库，效率相对较低。
 - 裸文件备份：也叫物理备份。拷贝数据库的物理文件，数据库既可以处于运行状态（mysqlhotcopy、ibbackup、xtrabackup一类工具），也可以处于停止状态，还原时间较短。，适用于大型数据库环境，但不能恢复到异构环境中，如windows。
 - 导出表：直接将表导入到文本文件中。
#### 按照备份数据库的内容分为
 - 完全备份：对数据库进行完整的备份。
 - 增量备份：在上一次完整备份的基础上，对更新的数据进行备份（xtrabackup）
 - 日志备份：二进制日志备份，主从同步。

### 工具：
1. mysqldump, 是逻辑备份工具，支持MyISAM和InnoDB引擎。数据库运行时，MyISAM，Aria引擎只支持温备，InnoDB支持热备和温备。单线程备份恢复较慢。
2. Xtrabackup(innobackupex工具)， 备份mysql大数据； InnoDB支持，增量备份；MyISAM温备，不支持增量。属于物理备份，速度快；
3. lvm-snapshot, 先请求全局锁，再创建快照，后释放全局锁;备份和恢复速度快。
4. 可以使用MySQL的 BACKUP TABLE 或 SELECT INTO OUTFILE 转储所
有数据到某个外部文件。这两条语句都接受将要创建的系统文件
名,此系统文件必须不存在,否则会出错。数据可以用 RESTORE
TABLE 来复原。


## 概念术语表

```
SQL (Structured Query Language) 结构化查询语言:
database 数据库
DBMS 数据库管理系统
table 表： 某种特定类型数据的结构化清单。
schema 模式： 关于数据库和表的布局及特性信息。
column 列：
datatype 数据类型：
row 行/record 记录：
primary key 主键：一列(或一组列),其值能够唯一区分表中每个行。
```

- 应该总是定义主键
主键的最好习惯：
1. 不更新主键列中的值;
2. 不重用主键列的值;
3. 不在主键列中使用可能会更改的值。