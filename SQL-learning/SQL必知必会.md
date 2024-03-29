# explain查看执行计划

## 01.
``` stylus
	DDL,DML,DCL,DQL 
	Navicat设计ER图 
	表名、表别名、字段名、字段别名等都小写；
	SQL 保留字、函数名、绑定变量等都大写
``` 
![Image of xmind](https://github.com/xaoduer/learning/blob/master/SQL-learning/SQL-png1/01.xmind.png)
## 02.
![Image of xmind](https://github.com/xaoduer/learning/blob/master/SQL-learning/SQL-png1/02.xmind.png)
## 03.
![Image of xmind](https://github.com/xaoduer/learning/blob/master/SQL-learning/SQL-png1/03.xmind.png)
## 04. DDL
``` stylus
	唯一索引和普通索引的区别在于它对字段进行了唯一性的约束
	索引方式：BTREE , HASH
	字段约束：主键、外键、唯一性、NOT NULL、DEFAULT、CHECK约束
```    
![Image of xmind](https://github.com/xaoduer/learning/blob/master/SQL-learning/SQL-png1/04.xmind.png)
## 05. SELECT
``` stylus
	Oracle排序: SELECT name, hp_max FROM (SELELECT name,hp_max FROM heros ORDER BY hp_max) WHERE ROWNUM <= 5
	关键字顺序：SELECT ... FROM ... WHERE ... GROUP BY ... HAVING ... 0RDER BY ...
	执行顺序：FROM > WHERE > GROUP BY > HAVING > SELECT > DISTINCT > ORDER BY > LIMIT 
``` 
![Image of xmind](https://github.com/xaoduer/learning/blob/master/SQL-learning/SQL-png1/05.xmind.png)
## 06. WHERE 数据过滤，尽量少用 LIKE '%太'
![Image of xmind](https://github.com/xaoduer/learning/blob/master/SQL-learning/SQL-png1/06.xmind.png)
## 07. SQL函数
``` stylus
	算数函数
	字符串函数
	日期函数
	转换函数
``` 	
![Image of xmind](https://github.com/xaoduer/learning/blob/master/SQL-learning/SQL-png1/07.xmind.png)
## 08. 聚集函数：输入一组数据的集合，输出单个值
``` stylus
	COUNT()
	MAX()
	MIN()
	SUM()
	AVG()
	WHERE和HAVING都是过滤，HAVING作用于分组
	SQL: SELECT COUNT(*) as num, role_main, role_assist FROM heros GROUP BY role_main, role_assist HAVING num > 5 ORDER BY num DESC
	SQL: SELECT COUNT(*) as num, role_main, role_assist FROM heros WHERE hp_max > 6000 GROUP BY role_main, role_assist HAVING num > 5 ORDER BY num DESC
```
![Image of xmind](https://github.com/xaoduer/learning/blob/master/SQL-learning/SQL-png1/08.xmind.png)
## 09. 子查询：从查询结果集中再次进行查询 EXISTS IN SOME ANY ALL
``` stylus
	非关联子查询：SELECT player_name, height FROM player WHERE height = (SELECT max(height) FROM player)
	关联子查询：SELECT player_name, height, team_id FROM player AS a WHERE height > (SELECT avg(height) FROM player AS b WHERE a.team_id = b.team_id)
	EXISTS子查询：SQL：SELECT player_id, team_id, player_name FROM player WHERE EXISTS (SELECT player_id FROM player_score WHERE player.player_id = player_score.player_id)
	SQL: SELECT player_id, team_id, player_name FROM player WHERE NOT EXISTS (SELECT player_id FROM player_score WHERE player.player_id = player_score.player_id)
	集合比较子查询 IN ANY ALL SOME
	SQL: SELECT player_id, team_id, player_name FROM player WHERE player_id in (SELECT player_id FROM player_score WHERE player.player_id = player_score.player_id)
	SQL: SELECT player_id, player_name, height FROM player WHERE height > ANY (SELECT height FROM player WHERE team_id = 1002)
	SQL: SELECT player_id, player_name, height FROM player WHERE height > ALL (SELECT height FROM player WHERE team_id = 1002)
	SQL: SELECT team_name, (SELECT count(*) FROM player WHERE player.team_id = team.team_id) AS player_num FROM team
``` 
![Image of xmind](https://github.com/xaoduer/learning/blob/master/SQL-learning/SQL-png1/09.xmind.png)
## 12. 视图VIEW
``` stylus
	CREATE VIEW view_name AS
	SELECT column1, column2
	FROM table
	WHERE condition
``` 
![Image of xmind](https://github.com/xaoduer/learning/blob/master/SQL-learning/SQL-png1/12.xmind.png)
## 13. 存储过程 Stored Procedure
``` stylus
	DELIMITER //
	CREATE PROCEDURE `get_hero_scores`(
       OUT max_max_hp FLOAT,
       OUT min_max_mp FLOAT,
       OUT avg_max_attack FLOAT,  
       s VARCHAR(255)
       )
	BEGIN
       SELECT MAX(hp_max), MIN(mp_max), AVG(attack_max) FROM heros WHERE role_main = s INTO max_max_hp, min_max_mp, avg_max_attack;
	END
	DELIMITER ;

CALL get_hero_scores(@max_max_hp, @min_max_mp, @avg_max_attack, '战士');
SELECT @max_max_hp, @min_max_mp, @avg_max_attack;
``` 
![Image of xmind](https://github.com/xaoduer/learning/blob/master/SQL-learning/SQL-png1/13.xmind.png)

## 14.事务处理
1.A，A，也就是原子性（Atomicity）。原子的概念就是不可分割，你可以把它理解为组成物质的基本单位  
2.C，一致性，Consistency，事务提交前后，数据库的完整性约束不能被破坏  
3.I，Isolation，一个事务在提交之前，对其它事务都是不可见的  
4.D,持久性Durability,事务提交后对数据的修改是持久性的，保存到日志或者磁盘  
![xmid](https://github.com/xaoduer/learning/blob/master/SQL-learning/SQL-png1/14.xmind.png)

## 15.事务隔离级别

![xmid](https://github.com/xaoduer/learning/blob/master/SQL-learning/SQL-png1/15.xmind.png)

	
	