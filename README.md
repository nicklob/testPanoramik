# Тестовое задание от Panoramik

Создаём таблицу purchases_dist:
```SQL
CREATE TABLE `purchases_dist` (
  `profile_id` varchar(255) NOT NULL,
  `date` datetime NOT NULL,
  `lot_string_id` varchar(255) NOT NULL,
  `price_usd` double DEFAULT NULL
) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```


Создаём таблицу profile_status_dist:
```SQL
CREATE TABLE `profile_status_dist` (
  `profile_id` varchar(255) NOT NULL,
  `install_day` datetime NOT NULL,
  `qa` bigint(20) NOT NULL,
  UNIQUE KEY `profile_id` (`profile_id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```

Заносим данные в таблицу purchases_dist:
```SQL
INSERT INTO `purchases_dist` VALUES ('profile_id_1','2021-09-23 00:00:00','a',0.99),('profile_id_1','2021-09-24 00:00:00','b',4.99),('profile_id_2','2021-09-25 00:00:00','a',0.99),('profile_id_3','2021-09-26 00:00:00','b',4.99),('profile_id_3','2021-09-27 00:00:00','a',0.99),('profile_id_3','2021-09-28 00:00:00','b',4.99),('profile_id_3','2021-09-29 00:00:00','a',19.99),('profile_id_4','2021-09-30 00:00:00','b',4.99),('profile_id_5','2021-10-01 00:00:00','a',0.99),('profile_id_5','2021-10-02 00:00:00','b',4.99),('profile_id_5','2021-10-03 00:00:00','a',0.99),('profile_id_5','2021-10-04 00:00:00','b',4.99),('profile_id_14','2021-09-25 00:00:00','a',49.99),('profile_id_15','2021-09-28 00:00:00','b',0.99),('profile_id_15','2021-09-28 00:00:00','a',0.99),('profile_id_15','2021-09-28 00:00:00','b',0.99),('profile_id_3','2021-10-03 00:00:00','a',9.99),('profile_id_3','2021-10-04 00:00:00','b',0.99),('profile_id_3','2021-09-25 00:00:00','a',0.99),('profile_id_1','2021-10-03 00:00:00','b',99.99),('profile_id_2','2021-10-04 00:00:00','a',0.99),('profile_id_3','2021-09-25 00:00:00','b',49.99),('profile_id_4','2021-10-02 00:00:00','b',4.99),('profile_id_4','2021-10-02 00:00:00','a',49.99);
```

Заносим данные в таблицу profile_status_dist:
```SQL
INSERT INTO `profile_status_dist` VALUES ('profile_id_1','2021-09-23 00:00:00',0),('profile_id_2','2021-09-25 00:00:00',0),('profile_id_3','2021-09-26 00:00:00',0),('profile_id_4','2021-10-01 00:00:00',0),('profile_id_5','2021-10-01 00:00:00',1),('profile_id_6','2021-09-25 00:00:00',0),('profile_id_7','2021-09-26 00:00:00',0),('profile_id_8','2021-09-23 00:00:00',1),('profile_id_9','2021-09-22 00:00:00',0),('profile_id_10','2021-09-21 00:00:00',1),('profile_id_11','2021-09-20 00:00:00',1),('profile_id_12','2021-09-19 00:00:00',0),('profile_id_13','2021-09-18 00:00:00',0),('profile_id_14','2021-09-17 00:00:00',0),('profile_id_15','2021-09-16 00:00:00',1),('profile_id_16','2021-09-15 00:00:00',0);
```

SQL запрос для получения необходимой таблицы:
```SQL
SELECT purchases_count_for_player as purchases_count, COUNT(profile_id) as players, SUM(purchases_count_for_player) as transactions, ROUND(SUM(total_gross_for_user),2) as total_gross, ROUND(AVG(total_gross_for_user),2) as ARPPU, ROUND(SUM(total_gross_for_user)/SUM(purchases_count_for_player),2) as avg_check FROM (SELECT COUNT(lot_string_id) as purchases_count_for_player, profile_id, SUM(price_usd) as total_gross_for_user FROM purchases_dist WHERE profile_id IN (SELECT profile_id FROM profile_status_dist WHERE install_day>="2021-09-22" AND qa=0) GROUP BY profile_id ) money_from_user GROUP BY purchases_count_for_player;
```

| purchases_count | players | transactions | total_gross | ARPPU | avg_check |
|-----------------|---------|--------------|-------------|-------|-----------|
|               2 |       1 |            2 |        1.98 |  1.98 |      0.99 |
|               3 |       2 |            6 |      165.94 | 82.97 |     27.66 |
|               8 |       1 |            8 |       92.92 | 92.92 |     11.62 |
