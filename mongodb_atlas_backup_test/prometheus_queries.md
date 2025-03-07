# MongoDB Atlas Backup Prometheus Queries

## Basic Check Queries

### Check all available snapshots
```
mongodb_atlas_backup_snapshot_snapshot_completed
```

### Check the timestamp of recent backups
```
mongodb_atlas_backup_snapshot_created_timestamp
```

### Get the size of all backups
```
mongodb_atlas_backup_snapshot_storage_size_bytes
```

## Alert Conditions

### Check for backups in the last 24 hours
```
# Convert timestamp string to unix timestamp first
max by (cluster_name) (
  time() - strptime(mongodb_atlas_backup_snapshot_created_timestamp, "%Y-%m-%dT%H:%M:%SZ") < 86400
  and
  mongodb_atlas_backup_snapshot_snapshot_completed == 1
)
```

### Check if all daily backups completed successfully
```
count by (cluster_name) (
  mongodb_atlas_backup_snapshot_snapshot_completed{frequency_type="daily"} == 1
) > 0
```

### Check backup size changes
```
# Calculate size change percentage over time
(
  max by (cluster_name) (mongodb_atlas_backup_snapshot_storage_size_bytes)
  /
  max by (cluster_name) (mongodb_atlas_backup_snapshot_storage_size_bytes offset 7d)
) * 100 - 100
```