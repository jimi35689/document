# SQL操作
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

## 修改头像目录
```sql
UPDATE [QPAccountsDB].[dbo].[AccountsInfo] SET HeadImage = REPLACE(HeadImage, 'https://galkjdl.oss-cn-shenzhen.aliyuncs.com/Portrait/Portrait/', '改为实际访问目录地址')

UPDATE [QPAccountsDB].[dbo].[AndroidHeadImageStock] SET ImageName = REPLACE(ImageName, 'https://galkjdl.oss-cn-shenzhen.aliyuncs.com/Portrait/Portrait/', '改为实际访问目录地址')
```

## 清除所有体验卡信息的脚本

```sql
truncate table[QPNativeWebDB].[dbo].[RcardInfo]
```

**警告！**
此操作是直接清空数据库，将清除所有验收卡。并不会生成操作员的操作日志，将无法追溯操作记录。

## 修改游戏场的准入金额

```sql
-----------------------以二人牛牛为例----------------------------
DECLARE
	@MinTableScore1 INT = 500,		--初级场  此输入是分值，等价为2.00元
	@MinTableScore2 INT = 2000,		--中级场
	@MinTableScore3 INT = 20000,	--高级场
	@MinTableScore4 INT = 100000	--至尊场


UPDATE [QPPlatformDB].[dbo].[GameRoomInfo] SET MinTableScore = @MinTableScore1,MinEnterScore = @MinTableScore1 WHERE ServerID = 51
UPDATE [QPPlatformDB].[dbo].[GameRoomInfo] SET MinTableScore = @MinTableScore2,MinEnterScore = @MinTableScore2 WHERE ServerID = 52
UPDATE [QPPlatformDB].[dbo].[GameRoomInfo] SET MinTableScore = @MinTableScore3,MinEnterScore = @MinTableScore3 WHERE ServerID = 53
UPDATE [QPPlatformDB].[dbo].[GameRoomInfo] SET MinTableScore = @MinTableScore4,MinEnterScore = @MinTableScore4 WHERE ServerID = 54
```

## 修改机器人VIP等级

```sql
---------------------------------------------------------------------------------------
--下列是设置VIP概率，全部概率加起来等于1
/*
	当前比例
	VIP0：0%
	VIP1：2% 
	VIP2：4% 
	VIP3：4% 
	VIP4：20% 
	VIP5：20% 
	VIP6：20% 
	VIP7：20%
	VIP8：4.5% 
	VIP9：4.5%
	VIP10：1% 
	*/
DECLARE
	--@VIP0 float = 0.0,
	--@VIP1 float = 0.02,
	--@VIP2 float = 0.04,
	--@VIP3 float = 0.04,
	--@VIP4 float = 0.2,
	--@VIP5 float = 0.2,
	--@VIP6 float = 0.2,
	--@VIP7 float = 0.2,
	--@VIP8 float = 0.045,
	--@VIP9 float = 0.045,
	--@VIP10 float =0.01
-- 现业务需要把AI VIP 全设置成 vip 0 所以 @VIP0 设置 1
	@VIP0 float =1,
	@VIP1 float = 0,
	@VIP2 float = 0,
	@VIP3 float = 0,
	@VIP4 float = 0,
	@VIP5 float = 0,
	@VIP6 float = 0,
	@VIP7 float = 0,
	@VIP8 float = 0,
	@VIP9 float = 0,
	@VIP10 float =0

-- 给实际阶梯概率
/*
	vip0:0.00
	vip1:0.02
	vip2:0.06
	vip3:0.10
	vip4:0.3
	vip5:0.5
	vip6:0.7
	vip7:0.9
	vip8:0.945
	vip9:0.990
	vip10:1.00 
*/
SET @VIP1 = @VIP0 + @VIP1
SET @VIP2 = @VIP1 + @VIP2
SET @VIP3 = @VIP2 + @VIP3
SET @VIP4 = @VIP3 + @VIP4
SET @VIP5 = @VIP4 + @VIP5
SET @VIP6 = @VIP5 + @VIP6
SET @VIP7 = @VIP6 + @VIP7
SET @VIP8 = @VIP7 + @VIP8
SET @VIP9 = @VIP8 + @VIP9
SET @VIP10 = @VIP9 + @VIP10

---------------------------------------------------------------------------------------

DECLARE @temp TABLE
(
      UserID INT
);

-- 将源表中的数据插入到表变量中
INSERT INTO @temp(UserID)
SELECT UserID FROM [QPAccountsDB].[dbo].[AndroidLockInfo]
ORDER BY UserID;

-- 声明变量
DECLARE
     @UserID INT,
	 @iMemberOrder int = 0,
	 @dMerberOrderPr float

     
 WHILE EXISTS(SELECT UserID FROM @temp)
 BEGIN
	set @dMerberOrderPr=RAND();
    set @iMemberOrder=0;

	if @dMerberOrderPr <= @VIP0
		begin
			set @iMemberOrder=0
		end
	else if @dMerberOrderPr <= @VIP1
		begin
			set @iMemberOrder=1
		end
	else if @dMerberOrderPr<=@VIP2
		begin
			set @iMemberOrder=2
		end
    else if @dMerberOrderPr<=@VIP3
		begin
			set @iMemberOrder=3
		end
  else if @dMerberOrderPr<=@VIP4
		begin
			set @iMemberOrder=4
		end 
  else if @dMerberOrderPr<=@VIP5
		begin
			set @iMemberOrder=5
		end
  else if @dMerberOrderPr<=@VIP6
		begin
			set @iMemberOrder=6
		end
  else if @dMerberOrderPr<=@VIP7
		begin
			set @iMemberOrder=7
		end
 else if @dMerberOrderPr<=@VIP8
		begin
			set @iMemberOrder=8
		end
 else if @dMerberOrderPr<=@VIP9
		begin
			set @iMemberOrder=9
		end
 else if @dMerberOrderPr<=@VIP10
		begin
			set @iMemberOrder=10
END
    SET ROWCOUNT 1
    SELECT @UserID = UserID FROM @temp;
    UPDATE [QPAccountsDB].[dbo].[AccountsInfo] SET MemberOrder = @iMemberOrder WHERE UserID = @UserID and IsAndroid = 1
	UPDATE [QPAccountsDB].[dbo].[AndroidLockInfo] SET MemberOrder = @iMemberOrder WHERE UserID = @UserID
    SET ROWCOUNT 0
     
    DELETE FROM @temp WHERE UserID = @UserID;
 END
```

