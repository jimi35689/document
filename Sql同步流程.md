# Sql同步流程

## 停止所有服务

## 备份

```sql
backup database QPAccountsDB to disk='d:\DB\QPAccountsDB.bak'
backup database QPAgencyDB to disk='d:\DB\QPAgencyDB.bak'
backup database QPEducateDB to disk='d:\DB\QPEducateDB.bak'
backup database QPGameMatchDB to disk='d:\DB\QPGameMatchDB.bak'
backup database QPGameScoreDB to disk='d:\DB\QPGameScoreDB.bak'
backup database QPNativeWebDB to disk='d:\DB\QPNativeWebDB.bak'
backup database QPPlatformDB to disk='d:\DB\QPPlatformDB.bak'
backup database QPRecordDB to disk='d:\DB\QPRecordDB.bak'
backup database QPRiskManagerDB to disk='d:\DB\QPRiskManagerDB.bak'
backup database QPTreasureDB to disk='d:\DB\QPTreasureDB.bak'
backup database HistoryBackupsDB to disk='d:\DB\HistoryBackupsDB.bak'
backup database QPWebIMDB to disk='d:\DB\QPWebIMDB.bak'
```

## 恢复

脚本

```sql
restore database QPAccountsDB from  disk = N'd:\DB\QPAccountsDB.bak'with file = 1, nounload, replace, stats = 10
restore database QPAgencyDB from  disk = N'd:\DB\QPAgencyDB.bak'with file = 1, nounload, replace, stats = 10
restore database QPEducateDB from  disk = N'd:\DB\QPEducateDB.bak'with file = 1, nounload, replace, stats = 10
restore database QPGameMatchDB from  disk = N'd:\DB\QPGameMatchDB.bak'with file = 1, nounload, replace, stats = 10
restore database QPGameScoreDB from  disk = N'd:\DB\QPGameScoreDB.bak'with file = 1, nounload, replace, stats = 10
restore database QPNativeWebDB from  disk = N'd:\DB\QPNativeWebDB.bak'with file = 1, nounload, replace, stats = 10
restore database QPPlatformDB from  disk = N'd:\DB\QPPlatformDB.bak'with file = 1, nounload, replace, stats = 10
restore database QPRecordDB from  disk = N'd:\DB\QPRecordDB.bak'with file = 1, nounload, replace, stats = 10
restore database QPRiskManagerDB from  disk = N'd:\DB\QPRiskManagerDB.bak'with file = 1, nounload, replace, stats = 10
restore database QPTreasureDB from  disk = N'd:\DB\QPTreasureDB.bak'with file = 1, nounload, replace, stats = 10
restore database HistoryBackupsDB from  disk = N'd:\DB\HistoryBackupsDB.bak'with file = 1, nounload, replace, stats = 10
restore database QPWebIMDB from  disk = N'd:\DB\QPWebIMDB.bak'with file = 1, nounload, replace, stats = 10
```

恢复后需运行google云盘里的环境sql脚本