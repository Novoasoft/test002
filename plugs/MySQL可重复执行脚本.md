### MySQL可重复执行脚本

#### 1. 建表：

```sql
DROP TABLE IF EXISTS tbl_user;
CREATE TABLE tbl_user (
  id int(11) NOT NULL AUTO_INCREMENT,
  name varchar(255) NOT NULL DEFAULT '',
  age int(4) NOT NULL DEFAULT '0',
  PRIMARY KEY (`id`)
);
```

#### 2. 添加列：

```sql
DROP PROCEDURE IF EXISTS add_column_if;
CREATE PROCEDURE add_column_if() 
BEGIN
	DECLARE
		iCnt  INT;
	SELECT
		count(*) INTO iCnt 
	FROM
		information_schema.COLUMNS 
	WHERE
		table_schema = ( SELECT DATABASE()) 
		AND table_name = 'tbl_user' 
		AND column_name = 'address';
	IF(iCnt = 0) THEN
			ALTER TABLE tbl_user add COLUMN address varchar(200) not null comment '地址';	
	END IF;
END;
CALL add_column_if;
DROP PROCEDURE add_column_if;
```

#### 3.删除列：

```
DROP PROCEDURE IF EXISTS del_column_if;
CREATE PROCEDURE del_column_if() 
BEGIN
	DECLARE
		iCnt  INT;
	SELECT
		count(*) INTO iCnt 
	FROM
		information_schema.COLUMNS 
	WHERE
		table_schema = ( SELECT DATABASE()) 
		AND table_name = 'tbl_user' 
		AND column_name = 'address';
	IF(iCnt = 1) THEN
			ALTER TABLE tbl_user  DROP COLUMN address;	
	END IF;
END;
CALL del_column_if;
DROP PROCEDURE del_column_if;
```

#### 4.修改：

```
-- 修改列名
DROP PROCEDURE IF EXISTS change_column_if;
CREATE PROCEDURE change_column_if() 
BEGIN
	DECLARE
		iCnt  INT;
	SELECT
		count(*) INTO iCnt 
	FROM
		information_schema.COLUMNS 
	WHERE
		table_schema = ( SELECT DATABASE()) 
		AND table_name = 'tbl_user' 
		AND column_name = 'address';
	IF(iCnt = 1) THEN
			ALTER TABLE tbl_user  change  column address  myaddress varchar(300);
	END IF;
END;
CALL change_column_if;
DROP PROCEDURE change_column_if;

-- 修改列属性
DROP PROCEDURE IF EXISTS modify_column_if;
CREATE PROCEDURE modify_column_if() 
BEGIN
	DECLARE
		iCnt  INT;
	SELECT
		count(*) INTO iCnt 
	FROM
		information_schema.COLUMNS 
	WHERE
		table_schema = ( SELECT DATABASE()) 
		AND table_name = 'tbl_user' 
		AND column_name = 'myaddress';
	IF(iCnt = 1) THEN
			ALTER TABLE tbl_user  MODIFY myaddress varchar(20) not null;
	END IF;
END;
CALL modify_column_if;
DROP PROCEDURE modify_column_if;
```

#### 5.添加和删除索引：

```
-- 添加索引
DROP PROCEDURE IF EXISTS add_index_if;
CREATE PROCEDURE add_index_if() 
BEGIN
	DECLARE
		iCnt  INT;
	SELECT
		count(*) INTO iCnt 
	FROM
		information_schema.statistics 
	WHERE
		table_schema = ( SELECT DATABASE()) 
		AND table_name = 'tbl_user' 
		AND index_name  = 'idx_tab_user';
	IF(iCnt = 0) THEN
			ALTER TABLE tbl_user ADD INDEX idx_tab_user(id,name);
	END IF;
END;
CALL add_index_if;
DROP PROCEDURE add_index_if;

-- 删除索引
DROP PROCEDURE IF EXISTS drop_index_if;
CREATE PROCEDURE drop_index_if() 
BEGIN
	DECLARE
		iCnt  INT;
	SELECT
		count(*) INTO iCnt 
	FROM
		information_schema.statistics 
	WHERE
		table_schema = ( SELECT DATABASE()) 
		AND table_name = 'tbl_user' 
		AND index_name  = 'idx_tab_user';
	IF(iCnt > 0) THEN
			ALTER TABLE tbl_user DROP INDEX idx_tab_user;
	END IF;
END;
CALL drop_index_if;
DROP PROCEDURE drop_index_if;
```

#### 6.添加删除约束：

```
-- 添加约束
DROP PROCEDURE IF EXISTS add_constraint_if;
CREATE PROCEDURE add_constraint_if() 
BEGIN
	DECLARE
		iCnt  INT;
	SELECT
		count(*) INTO iCnt 
	FROM
		information_schema.TABLE_CONSTRAINTS 
	WHERE
		table_schema = ( SELECT DATABASE()) 
		AND table_name = 'tbl_user' 
		AND constraint_name  = 'un_tab_user_id_name';
	IF(iCnt = 0) THEN
			alter table tbl_user add unique un_tab_user_id_name (id,name);
	END IF;
END;
CALL add_constraint_if;
DROP PROCEDURE add_constraint_if;

```

#### 删除表

```
DROP TABLE IF EXISTS tbl_user;
```

