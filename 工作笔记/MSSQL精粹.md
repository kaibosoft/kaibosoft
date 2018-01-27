### 日期处理
    SELECT CONVERT(varchar(100), GETDATE(), 0): 05 16 2006 10:57AM
    SELECT CONVERT(varchar(100), GETDATE(), 1): 05/16/06
    SELECT CONVERT(varchar(100), GETDATE(), 2): 06.05.16
    SELECT CONVERT(varchar(100), GETDATE(), 3): 16/05/06
    SELECT CONVERT(varchar(100), GETDATE(), 4): 16.05.06
    SELECT CONVERT(varchar(100), GETDATE(), 5): 16-05-06
    SELECT CONVERT(varchar(100), GETDATE(), 6): 16 05 06
    SELECT CONVERT(varchar(100), GETDATE(), 7): 05 16, 06
    SELECT CONVERT(varchar(100), GETDATE(), 8): 10:57:46
    SELECT CONVERT(varchar(100), GETDATE(), 9): 05 16 2006 10:57:46:827AM
    SELECT CONVERT(varchar(100), GETDATE(), 10): 05-16-06
    SELECT CONVERT(varchar(100), GETDATE(), 11): 06/05/16
    SELECT CONVERT(varchar(100), GETDATE(), 12): 060516
    SELECT CONVERT(varchar(100), GETDATE(), 13): 16 05 2006 10:57:46:937
    SELECT CONVERT(varchar(100), GETDATE(), 14): 10:57:46:967
    SELECT CONVERT(varchar(100), GETDATE(), 20): 2006-05-16 10:57:47
    SELECT CONVERT(varchar(100), GETDATE(), 21): 2006-05-16 10:57:47.157
    SELECT CONVERT(varchar(100), GETDATE(), 22): 05/16/06 10:57:47 AM
    SELECT CONVERT(varchar(100), GETDATE(), 23): 2006-05-16 /////////////////常用
    SELECT CONVERT(varchar(100), GETDATE(), 24): 10:57:47
    SELECT CONVERT(varchar(100), GETDATE(), 25): 2006-05-16 10:57:47.250
    SELECT CONVERT(varchar(100), GETDATE(), 100): 05 16 2006 10:57AM
    SELECT CONVERT(varchar(100), GETDATE(), 101): 05/16/2006
    SELECT CONVERT(varchar(100), GETDATE(), 102): 2006.05.16
    SELECT CONVERT(varchar(100), GETDATE(), 103): 16/05/2006
    SELECT CONVERT(varchar(100), GETDATE(), 104): 16.05.2006
    SELECT CONVERT(varchar(100), GETDATE(), 105): 16-05-2006
    SELECT CONVERT(varchar(100), GETDATE(), 106): 16 05 2006
    SELECT CONVERT(varchar(100), GETDATE(), 107): 05 16, 2006
    SELECT CONVERT(varchar(100), GETDATE(), 108): 10:57:49
    SELECT CONVERT(varchar(100), GETDATE(), 109): 05 16 2006 10:57:49:437AM
    SELECT CONVERT(varchar(100), GETDATE(), 110): 05-16-2006
    SELECT CONVERT(varchar(100), GETDATE(), 111): 2006/05/16
    SELECT CONVERT(varchar(100), GETDATE(), 112): 20060516
    SELECT CONVERT(varchar(100), GETDATE(), 113): 16 05 2006 10:57:49:513
    SELECT CONVERT(varchar(100), GETDATE(), 114): 10:57:49:547
    SELECT CONVERT(varchar(100), GETDATE(), 120): 2006-05-16 10:57:49
    SELECT CONVERT(varchar(100), GETDATE(), 121): 2006-05-16 10:57:49.700
    SELECT CONVERT(varchar(100), GETDATE(), 126): 2006-05-16T10:57:49.827
    SELECT CONVERT(varchar(100), GETDATE(), 130): 18 ???? ?????? 1427 10:57:49:907AM
    SELECT CONVERT(varchar(100), GETDATE(), 131): 18/04/1427 10:57:49:920AM
***
### 特殊日期获取
    select   dateadd(dd,-day(dateadd(month,-1,getdate()))+1,dateadd(month,-1,getdate())) /*上个月一号*/
    select   dateadd(dd,-day(getdate()),getdate())  /* 上月月底 */
    select   dateadd(dd,-day(getdate())+1,getdate())  /* 本月一号 */
    select   dateadd(dd,-day(dateadd(month,1,getdate())),dateadd(month,1,getdate()))/* 本月底 */
    select   dateadd(dd,-day(dateadd(month,1,getdate()))+1,dateadd(month,1,getdate()))/* 下月一号 */
    select   dateadd(dd,-day(dateadd(month,2,getdate())),dateadd(month,2,getdate()))/* 下月月底 */
***
### 数据整理技巧
    ltrim(rtrim(convert(varchar(38),cast(@data as real))))实现去掉小数点后面无用的零
    select right(10000000+49999,7)  0049999
