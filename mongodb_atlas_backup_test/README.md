# MongoDB Atlas Backup Monitoring with Prometheus

This configuration allows you to monitor MongoDB Atlas backup snapshots using Prometheus and the JSON Exporter.

## Setup Instructions

### Testing with Docker Compose

1. Run the Docker Compose stack to test with mock data:
   ```bash
   cd mongodb_atlas_backup_test
   docker-compose up -d
   ```

2. Access Prometheus at `http://localhost:9090` and try the queries from the `prometheus_queries.md` file

### Production Deployment

1. For production, copy and edit the provided configuration file:
   ```bash
   cp mongodb_atlas_backup_config_production.yml /etc/json_exporter/mongodb_atlas_backup.yml
   ```

2. Update the credentials:
   ```yaml
   http_client_config:
     basic_auth:
       username: "YOUR_ACCESS_KEY"
       password: "YOUR_SECRET_KEY"
   ```

3. Configure Prometheus to scrape from the JSON exporter with the MongoDB Atlas API endpoint:
   ```yaml
   - job_name: 'mongodb-atlas-backup'
     metrics_path: /probe
     params:
       module: [mongodb_atlas_backup]
       target: ['https://cloud.mongodb.com/api/atlas/v2/groups/YOUR_GROUP_ID/clusters/YOUR_CLUSTER_NAME/backup/snapshots/shardedClusters']
     static_configs:
       - targets: ['json-exporter:7979']
   ```

4. Import `prometheus_alerts.yml` to set up backup monitoring alerts

## Checking for Backups

The most important query is to check for recent backups (within 24 hours):

```
max by (cluster_name) (
  time() - strptime(mongodb_atlas_backup_snapshot_created_timestamp, "%Y-%m-%dT%H:%M:%SZ") < 86400
  and mongodb_atlas_backup_snapshot_snapshot_completed == 1
)
```

See `prometheus_queries.md` for additional monitoring queries.