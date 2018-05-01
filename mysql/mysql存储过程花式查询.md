## 统计时间段查询
```
  CREATE PROCEDURE PROC_REPORT_SELECT_COUNT(IN parlorId INT)#0:报告数，1:阅读量，2:点评量，3:顾客数
  BEGIN
    SELECT
      Today.detectionCount as today_detection,Today.viewCount as today_view,Today.reviewCount as today_reviewCount,Today.customerCount as today_customer,
      SevenDay.detectionCount as day7_detection,SevenDay.viewCount as day7_view,SevenDay.reviewCount as day7_reviewCount,SevenDay.customerCount as day7_customer,
      ThirtyDay.detectionCount as day30_detection,ThirtyDay.viewCount as day30_view,ThirtyDay.reviewCount as day30_reviewCount,ThirtyDay.customerCount as day30_customer,
      SixtyDay.detectionCount as day60_detection,SixtyDay.viewCount as day60_view,SixtyDay.reviewCount as day60_reviewCount,SixtyDay.customerCount as day60_customer,
      AllDay.detectionCount as allday_detection,AllDay.viewCount as allday_view,AllDay.reviewCount as allday_reviewCount,AllDay.customerCount as allday_customer
    FROM
    #查询今天的0:报告数，1:阅读量，2:点评量，3:顾客数
    (SELECT count(*) as detectionCount,sum(view) as viewCount,sum(review) as reviewCount,count(DISTINCT customer_id) as customerCount FROM detection
      WHERE parlor_id=parlorId AND update_time IS NOT NULL
      AND create_time>=CURRENT_DATE() AND create_time<DATE_ADD(CURRENT_DATE(),INTERVAL 1 DAY)) as Today,
    #查询最近7天的0:报告数，1:阅读量，2:点评量，3:顾客数
    (SELECT count(*) as detectionCount,sum(view) as viewCount,sum(review) as reviewCount,count(DISTINCT customer_id) as customerCount FROM detection
      WHERE parlor_id=parlorId AND update_time IS NOT NULL
      AND create_time>=DATE_SUB(CURRENT_DATE(),INTERVAL 1 DAY) AND create_time<DATE_ADD(CURRENT_DATE(),INTERVAL 1 DAY)) as SevenDay,
    #查询最近一个月的0:报告数，1:阅读量，2:点评量，3:顾客数
    (SELECT count(*) as detectionCount,sum(view) as viewCount,sum(review) as reviewCount,count(DISTINCT customer_id) as customerCount FROM detection
      WHERE parlor_id=parlorId AND update_time IS NOT NULL
      AND create_time>=DATE_SUB(CURRENT_DATE(),INTERVAL 1 MONTH) AND create_time<DATE_ADD(CURRENT_DATE(),INTERVAL 1 DAY)) as ThirtyDay,
    #查询最60天的0:报告数，1:阅读量，2:点评量，3:顾客数
    (SELECT count(*) as detectionCount,sum(view) as viewCount,sum(review) as reviewCount,count(DISTINCT customer_id) as customerCount FROM detection
      WHERE parlor_id=parlorId AND update_time IS NOT NULL
      AND create_time>=DATE_SUB(CURRENT_DATE(),INTERVAL 60 DAY) AND create_time<DATE_ADD(CURRENT_DATE(),INTERVAL 1 DAY)) as SixtyDay,
    #查询累计的0:报告数，1:阅读量，2:点评量，3:顾客数
    (SELECT count(*) as detectionCount,sum(view) as viewCount,sum(review) as reviewCount,count(DISTINCT customer_id) as customerCount FROM detection
      WHERE parlor_id=parlorId AND update_time IS NOT NULL
      AND create_time<DATE_ADD(CURRENT_DATE(),INTERVAL 1 DAY)) as AllDay;
  END;
```
<!--more-->
## 统计每天结果并存储到新表
```
CREATE PROCEDURE PROC_REPORT_CREATE_REPORT_SUBWHILE(IN parlorId INT)#针对单个美容院统计每天的各项数据
  BEGIN
    DECLARE i INT;
    SET i=2;#从昨天开始，直到最初那天
    WHILE (i<=200) DO
        INSERT INTO data_statics(create_time,report_count,view_count,review_count,customer_count,count_time,parlor_id)
          (SELECT NOW(),count(*),sum(view),sum(review),count(DISTINCT customer_id),DATE_SUB(CURRENT_DATE(),INTERVAL i-1 DAY),parlorId
            FROM detection
            WHERE parlor_id=parlorId AND update_time IS NOT NULL
            AND create_time>=DATE_SUB(CURRENT_DATE(),INTERVAL i-1 DAY) AND
            create_time<DATE_SUB(CURRENT_DATE(),INTERVAL i-2 DAY)
          );
        SET i=i+1;
    END WHILE;
  END;
```
```
CREATE PROCEDURE PROC_REPORT_CREATE_REPORT_MAINWHILE()#查询出所有美容院,然后调用上面的存储过程
  BEGIN
    DECLARE i INT;
    DECLARE j INT;
    SET i=0;
    SET j=(SELECT count(*) FROM parlor WHERE deleted=0);
    WHILE (i<j) DO
      CALL PROC_REPORT_CREATE_REPORT_SUBWHILE((SELECT id FROM parlor WHERE deleted=0 LIMIT i,1));
      SET i=i+1;
    END WHILE;
  END;
```
## 统计所有业务人员各种业务操作的数量
```
CREATE PROCEDURE PROC_STUFF_HANDLE_SELECT_COUNUT(IN parlorId INT,IN startDate DATE,IN endDate DATE)#某一美容院任意时间段所有业务员的各种业务量统计
  BEGIN
    #发送短信 更改标签 淘客 添加备注 潜客 评价报告
    SELECT t1.stuff_name,t1.amCount,t2.tkCount,t3.smsgCount,t4.etCount,t5.qiankeCount,t6.crCount
    FROM
    (SELECT sn.stuff_name,ifnull(addMark.amCount,0) AS amCount,ifnull(addMark.handle_detail,'添加备注') AS hd
     FROM
      (SELECT DISTINCT(stuff_name) AS stuff_name FROM stuff_handle_logs) AS sn LEFT JOIN
      (SELECT count(*) AS amCount,handle_detail,stuff_name
          FROM stuff_handle_logs
          WHERE parlor_id=parlorId AND handle_detail='添加备注' AND stuff_name IN (SELECT DISTINCT(stuff_name) FROM stuff_handle_logs)
            AND create_time>=startDate AND create_time<DATE_ADD(endDate,INTERVAL 1 DAY)
          GROUP BY stuff_name
      ) as addMark on sn.stuff_name=addMark.stuff_name
    ) as t1,
    (SELECT sn.stuff_name,ifnull(taoke.tkCount,0) AS tkCount,ifnull(taoke.handle_detail,'淘客') AS hd
     FROM
      (SELECT DISTINCT(stuff_name) AS stuff_name FROM stuff_handle_logs) AS sn LEFT JOIN
      (SELECT count(*) AS tkCount,handle_detail,stuff_name
          FROM stuff_handle_logs
          WHERE parlor_id=parlorId AND handle_detail='淘客' AND stuff_name IN (SELECT DISTINCT(stuff_name) FROM stuff_handle_logs)
            AND create_time>=startDate AND create_time<DATE_ADD(endDate,INTERVAL 1 DAY)
          GROUP BY stuff_name
      ) as taoke on sn.stuff_name=taoke.stuff_name
    ) as t2,
    (SELECT sn.stuff_name,ifnull(sendSMS.smsgCount,0) AS smsgCount,ifnull(sendSMS.handle_detail,'发送短信') AS hd
     FROM
      (SELECT DISTINCT(stuff_name) AS stuff_name FROM stuff_handle_logs) AS sn LEFT JOIN
      (SELECT count(*) AS smsgCount,handle_detail,stuff_name
          FROM stuff_handle_logs
          WHERE parlor_id=parlorId AND handle_detail='发送短信' AND stuff_name IN (SELECT DISTINCT(stuff_name) FROM stuff_handle_logs)
            AND create_time>=startDate AND create_time<DATE_ADD(endDate,INTERVAL 1 DAY)
          GROUP BY stuff_name
      ) as sendSMS on sn.stuff_name=sendSMS.stuff_name
    ) as t3,
    (SELECT sn.stuff_name,ifnull(editTag.etCount,0) AS etCount,ifnull(editTag.handle_detail,'更改标签') AS hd
     FROM
      (SELECT DISTINCT(stuff_name) AS stuff_name FROM stuff_handle_logs) AS sn LEFT JOIN
      (SELECT count(*) AS etCount,handle_detail,stuff_name
          FROM stuff_handle_logs
          WHERE parlor_id=parlorId AND handle_detail='更改标签' AND stuff_name IN (SELECT DISTINCT(stuff_name) FROM stuff_handle_logs)
            AND create_time>=startDate AND create_time<DATE_ADD(endDate,INTERVAL 1 DAY)
          GROUP BY stuff_name
      ) as editTag on sn.stuff_name=editTag.stuff_name
    ) as t4
    (SELECT sn.stuff_name,ifnull(qianke.qiankeCount,0) AS qiankeCount,ifnull(qianke.handle_detail,'潜客') AS hd
     FROM
      (SELECT DISTINCT(stuff_name) AS stuff_name FROM stuff_handle_logs) AS sn LEFT JOIN
      (SELECT count(*) AS qiankeCount,handle_detail,stuff_name
          FROM stuff_handle_logs
          WHERE parlor_id=parlorId AND handle_detail='潜客' AND stuff_name IN (SELECT DISTINCT(stuff_name) FROM stuff_handle_logs)
            AND create_time>=startDate AND create_time<DATE_ADD(endDate,INTERVAL 1 DAY)
          GROUP BY stuff_name
      ) as qianke on sn.stuff_name=qianke.stuff_name
    ) as t5,
    (SELECT sn.stuff_name,ifnull(commentReport.crCount,0) AS crCount,ifnull(commentReport.handle_detail,'评价报告') AS hd
     FROM
      (SELECT DISTINCT(stuff_name) AS stuff_name FROM stuff_handle_logs) AS sn LEFT JOIN
      (SELECT count(*) AS crCount,handle_detail,stuff_name
          FROM stuff_handle_logs
          WHERE parlor_id=parlorId AND handle_detail='评价报告' AND stuff_name IN (SELECT DISTINCT(stuff_name) FROM stuff_handle_logs)
            AND create_time>=startDate AND create_time<DATE_ADD(endDate,INTERVAL 1 DAY)
          GROUP BY stuff_name
      ) as commentReport on sn.stuff_name=commentReport.stuff_name
    ) as t6
    WHERE t1.stuff_name=t2.stuff_name AND t1.stuff_name=t3.stuff_name AND t1.stuff_name=t4.stuff_name AND t1.stuff_name=t5.stuff_name
          AND t1.stuff_name=t6.stuff_name AND t2.stuff_name=t3.stuff_name AND t2.stuff_name=t4.stuff_name AND t2.stuff_name=t5.stuff_name
          AND t2.stuff_name=t6.stuff_name AND t3.stuff_name=t4.stuff_name AND t3.stuff_name=t5.stuff_name AND t3.stuff_name=t6.stuff_name
          AND t4.stuff_name=t5.stuff_name AND t4.stuff_name=t6.stuff_name AND t5.stuff_name=t6.stuff_name;
  END;
```
## 定时任务每天凌晨3点统计昨天情况
```
CREATE PROCEDURE PROC_REPORT_CREATE_REPORT_SUBWHILE_YESTODY(IN parlorId INT)#针对单个美容院统计昨天的各项数据(event在凌晨执行)
  BEGIN
    INSERT INTO data_statics(create_time,report_count,view_count,review_count,customer_count,count_time,parlor_id)
      (SELECT NOW(),count(*),sum(view),sum(review),count(DISTINCT customer_id),DATE_SUB(CURRENT_DATE(),INTERVAL 1 DAY),parlorId
        FROM detection
        WHERE parlor_id=parlorId AND update_time IS NOT NULL
        AND create_time>=DATE_SUB(CURRENT_DATE(),INTERVAL 1 DAY) AND
        create_time<CURRENT_DATE()
      );
  END;
```
```
CREATE PROCEDURE PROC_REPORT_CREATE_REPORT_MAINWHILE_YESTODY()#查询出所有美容院,然后调用上面的存储过程(针对昨天的)
  BEGIN
    DECLARE i INT;
    DECLARE j INT;
    SET i=0;
    SET j=(SELECT count(*) FROM parlor WHERE deleted=0);
    WHILE (i<j) DO
      CALL PROC_REPORT_CREATE_REPORT_SUBWHILE_YESTODY((SELECT id FROM parlor WHERE deleted=0 LIMIT i,1));
      SET i=i+1;
    END WHILE;
  END;
```
```
CREATE EVENT EVENT_INSERT_YESTODAY_COUNT #每天凌晨3点执行一次
  ON SCHEDULE EVERY 1 DAY STARTS DATE_ADD(DATE(CURRENT_DATE() + 1),interval 3 hour)
  ON COMPLETION PRESERVE
  DO CALL PROC_REPORT_CREATE_REPORT_MAINWHILE_YESTODY();
```
## 定时任务状态查看
```
SHOW VARIABLES LIKE 'event_scheduler';
SET GLOBAL event_scheduler = 'ON';
```
## 存储过程调用
```
CALL PROC_REPORT_SELECT_COUNT(83);
CALL PROC_REPORT_CREATE_REPORT_MAINWHILE();#一次性的，以后用定时任务，在每天凌晨3点统计每个美容院前一天的数据即可
CALL PROC_REPORT_CREATE_REPORT_MAINWHILE_YESTODY();
CALL PROC_STUFF_HANDLE_SELECT_COUNUT(83,STR_TO_DATE('2017-08-01', '%Y-%m-%d'),STR_TO_DATE('2017-08-13', '%Y-%m-%d'));
```
## 另mysql系统日期函数
```
################操作日期2017-07-24#########
SELECT CURRENT_DATE(); #2017-07-24 当天日期
SELECT CURDATE(); #2017-07-24 当天日期
SELECT CURRENT_TIME(); #11:25:52 当前时间
SELECT CURTIME(); #11:25:52 当前时间
SELECT NOW(); #2017-07-24 11:25:52 当前 日期+时间
#SELECT DATE_SUB(date,INTERVAL int keyword);
SELECT DATE_SUB(CURRENT_DATE(),INTERVAL 1 DAY); #2017-07-23 昨天
SELECT DATE_SUB(CURRENT_DATE(),INTERVAL 1 MONTH); #2017-06-24 上个月这天
SELECT DATE_SUB(CURRENT_DATE(),INTERVAL 1 YEAR); #2016-07-24 去年今天
SELECT DATE_SUB(CURRENT_DATE(),INTERVAL '1-1' YEAR_MONTH); #2016-06-24 1年1个月以前
#SELECT DATE_ADD(date,INTERVAL int keyword);
SELECT DATE_ADD(CURRENT_DATE(),INTERVAL 1 DAY); #2017-07-25 明天
SELECT DATE_ADD(CURRENT_DATE(),INTERVAL 1 MONTH); #2017-08-24 下个月这天
SELECT DATE_ADD(CURRENT_DATE(),INTERVAL 1 YEAR); #2018-07-24 明年今天
SELECT DATE_ADD(CURRENT_DATE(),INTERVAL '1-1' YEAR_MONTH); #2018-08-24 1年1个月以后
################字符串转日期 2017-07-24#########
SELECT STR_TO_DATE('2016-01-02', '%Y-%m-%d');
```