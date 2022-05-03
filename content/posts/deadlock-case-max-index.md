---
title: "Deadlock Case Max Index"
date: 2022-05-04T00:12:27+08:00
draft: true
categories: ["MySQL"]
tags: ["lock","index"]
draft: true
comment: true
---

```
select * from performance_schema.data_locks
select * from performance_schema.innodb_locks
```

```

CREATE TABLE `t` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `c` int DEFAULT NULL,
  `d` int DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `id` (`id`),
  KEY `t_c_idx` (`c`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=26 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

INSERT INTO `t` (`id`, `c`, `d`) VALUES
(0, 0, 0),
(5, 5, 5),
(10, 10, 10),
(15, 15, 15),
(20, 20, 20),
(25, 25, 25);
(26, 0, 0);
(27, 0, 0);
```

| step  | connection 1 | connection 2 |
| --- | --- | --- |
| 1 | begin; | begin; |
| 2 | select MAX(c) from t for update |  |
| 3 |  | select MAX(c) from t for update (it will wait for c1) |
| 4 | update t set c = 26 where id = 26; ( it will wait for c2 and then deadlock ) |  |
| 5 |  | ## update t set c = 26 where id = 27;  |

this case that i retried when it found deadlock

## why does it found deadlock

step 2 would get (20 - 25] (gap lock) + 25 row lock   and supernum (25 ~ **∞**)

step 3 would wait for c1 because they would get the same key

step 4 it wanna update c = 26 ,but c2 waited for get lock after step 2

step 4 would wait for c2, then it was found deadlock
