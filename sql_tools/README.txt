The following tools have been posted to this folder:
	sql_cmd.py
	mysql419.py

These tools must be run from an engineering server (e.g. FLIP).

<Tool> sql_cmd.py

	This tool implements a MySQL command on the CS419 group8 database.
	It is a self-contained function meant to be called by other scripts.

	To use this tool within another python script: 
		import sql_cmd
		syntax: 'sql_cmd.execute(query, borders)' where 
			query = MySQL query text string
			borders = (True = table borders; False = data output only) 
		example: assigning sql_cmd result to variable sql_result:
			sql_result = sql_cmd.execute(query, False)

<Tool> mysql419.py

	To RUN this tool, type at the command prompt:
		python mysql419.py

	This tool emulates a live MySQL console applied to the cs419-g8 database.
	At runtime, user gets a prompt 'mysql419 >> '
	Enter a MySQL query at the prompt.

	Any input containing ';' will mark the end of a query and be executed.
	Until an input contains ';' any new text is added to a query being constructed.
	You may add multi-line queries and multiple queries at the same time.  The 
	program will execute the queries sequentially.

	To EXIT, enter 'exit' at the prompt.

<EXAMPLE> Below is text from using sql_cmd.py

	% python

	>>> my_query = 'SELECT * FROM appointment;'
	>>> import sql_cmd
	>>>
	>>> sql_cmd.execute(my_query)
	'+----+--------------+--------------------+---------------------------+-------------------------------+------------------+------------------------+----------------------+---------------------+\n| id | advisor_name | student_name       | advisor_email             | student_email                 | appointment_date | appointment_start_time | appointment_end_time | date_created        |\n+----+--------------+--------------------+---------------------------+-------------------------------+------------------+------------------------+----------------------+---------------------+\n|  1 | Jeff Rix     | Rittie Chuaprasert | rixj@onid.oregonstate.edu | chuaprar@onid.oregonstate.edu | 2015-02-09       | 11:30:00               | 12:00:00             | 2015-02-04 22:40:43 |\n|  3 | Jeff Rix     | Rittie Chuaprasert | rixj@onid.oregonstate.edu | chuaprar@onid.oregonstate.edu | 2015-02-09       | 11:30:00               | 12:00:00             | 2015-02-06 01:42:51 |\n+----+--------------+--------------------+---------------------------+-------------------------------+------------------+------------------------+----------------------+---------------------+\n'
	>>>
	>>> sql_cmd.execute(my_query, False)
	'1\tJeff Rix\tRittie Chuaprasert\trixj@onid.oregonstate.edu\tchuaprar@onid.oregonstate.edu\t2015-02-09\t11:30:00\t12:00:00\t2015-02-04 22:40:43\n3\tJeff Rix\tRittie Chuaprasert\trixj@onid.oregonstate.edu\tchuaprar@onid.oregonstate.edu\t2015-02-09\t11:30:00\t12:00:00\t2015-02-06 01:42:51\n'
	>>> 

<EXAMPLE> Below is text from Linux command line session using mysql419.py

	flip3 ~/CS419 22% python mysql419.py
	mysql419 >>show databases;
	USE cs419-g8;
	SET FOREIGN_KEY_CHECKS=0;
	DROP TABLE IF EXISTS 'test_create_table';
	CREATE TABLE 'test_create_table'(
	'id' INT(11) AUTO_INCREMENT,
	'name' VARCHAR(255),
	PRIMARY KEY ('id')
	) ENGINE=InnoDB;
	show tables;

	(query >> 'show databases;')
	+--------------------+
	| Database           |
	+--------------------+
	| information_schema |
	| cs419-g8           |
	+--------------------+

	mysql419 >>(query >> 'USE cs419-g8;')

	mysql419 >>(query >> 'SET FOREIGN_KEY_CHECKS=0;')

	mysql419 >>(query >> 'DROP TABLE IF EXISTS 'test_create_table';')

	mysql419 >>>>>>>>>>(query >> 'CREATE TABLE 'test_create_table'('id' INT(11) AUTO_INCREMENT,'name' VARCHAR(255),PRIMARY KEY ('id')) ENGINE=InnoDB;')

	mysql419 >>(query >> 'show tables;')
	+--------------------+
	| Tables_in_cs419-g8 |
	+--------------------+
	| test_create_table  |
	+--------------------+

	mysql419 >>SHOW CREATE TABLE 'test_create_table';
	(query >> 'SHOW CREATE TABLE 'test_create_table';')
	+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
	| Table             | Create Table                                                                                                                                                               |
	+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
	| test_create_table | CREATE TABLE `test_create_table` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(255) DEFAULT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=latin1 |
	+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

	mysql419 >>SHOW COLUMNS IN 'test_create_table';
	(query >> 'SHOW COLUMNS IN 'test_create_table';')
	+-------+--------------+------+-----+---------+----------------+
	| Field | Type         | Null | Key | Default | Extra          |
	+-------+--------------+------+-----+---------+----------------+
	| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
	| name  | varchar(255) | YES  |     | NULL    |                |
	+-------+--------------+------+-----+---------+----------------+

	mysql419 >>SHOW COLUMNS FROM 'test_create_table';
	(query >> 'SHOW COLUMNS FROM 'test_create_table';')
	+-------+--------------+------+-----+---------+----------------+
	| Field | Type         | Null | Key | Default | Extra          |
	+-------+--------------+------+-----+---------+----------------+
	| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
	| name  | varchar(255) | YES  |     | NULL    |                |
	+-------+--------------+------+-----+---------+----------------+

	mysql419 >>SELECT * FROM 'test_create_table';
	(query >> 'SELECT * FROM 'test_create_table';')

	mysql419 >>INSERT INTO 'test_create_table' ('name') VALUES ("Kevin McGrath");
	(query >> 'INSERT INTO 'test_create_table' ('name') VALUES ("Kevin McGrath");')

	mysql419 >>SELECT * FROM 'test_create_table';
	(query >> 'SELECT * FROM 'test_create_table';')
	+----+---------------+
	| id | name          |
	+----+---------------+
	|  1 | Kevin McGrath |
	+----+---------------+

	mysql419 >>exit
	flip3 ~/CS419 23% 

<EXAMPLE#2> Program augmented to handle standard MySQL comments 
('--', '#', '/*'..'*/)

### With a Few Comments ###

	% python ~/CS419/mysql419.py
	mysql419 >>SET FOREIGN_KEY_CHECKS=0;
	DROP TABLE IF EXISTS `appointment`;

	-- Create a table to hold the appointments
	CREATE TABLE appointment (
	`id` int NOT NULL AUTO_INCREMENT,
	`advisor_name` varchar(100),
	`student_name` varchar(100),
	`advisor_email` varchar(100),
	`student_email` varchar(100),
	`appointment_date` date,
	`appointment_start_time` time,
	`appointment_end_time` time,
	`date_created` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
	PRIMARY KEY (`id`)
	) ENGINE=InnoDB;

	-- Prefill Data
	INSERT appointment (advisor_name, student_name,
	advisor_email, student_email, appointment_date, appointment_start_time,
	appointment_end_time)
	VALUES ("Jeff Rix", "Rittie Chuaprasert",
	"rixj@onid.oregonstate.edu", "chuaprar@onid.oregonstate.edu",
	"2015-02-09", "11:30:00", "12:00:00");

	-- View initial Data
	SELECT * FROM appointment;

	(query >> 'SET FOREIGN_KEY_CHECKS=0;')

	mysql419 >>(query >> 'DROP TABLE IF EXISTS `appointment`;')

	mysql419 >>>>>>>>>>>>>>>>>>>>>>>>>>>>(query >> 'CREATE TABLE appointment (`id` int NOT NULL AUTO_INCREMENT,`advisor_name` varchar(100),`student_name` varchar(100),`advisor_email` varchar(100),`student_email` varchar(100),`appointment_date` date,`appointment_start_time` time,`appointment_end_time` time,`date_created` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,PRIMARY KEY (`id`)) ENGINE=InnoDB;')

	mysql419 >>>>>>>>>>>>>>>>(query >> 'INSERT appointment (advisor_name, student_name,advisor_email, student_email, appointment_date, appointment_start_time,appointment_end_time)VALUES ("Jeff Rix", "Rittie Chuaprasert","rixj@onid.oregonstate.edu", "chuaprar@onid.oregonstate.edu","2015-02-09", "11:30:00", "12:00:00");')

	mysql419 >>>>>>(query >> 'SELECT * FROM appointment;')
	+----+--------------+--------------------+---------------------------+-------------------------------+------------------+------------------------+----------------------+---------------------+
	| id | advisor_name | student_name       | advisor_email             | student_email                 | appointment_date | appointment_start_time | appointment_end_time | date_created        |
	+----+--------------+--------------------+---------------------------+-------------------------------+------------------+------------------------+----------------------+---------------------+
	|  1 | Jeff Rix     | Rittie Chuaprasert | rixj@onid.oregonstate.edu | chuaprar@onid.oregonstate.edu | 2015-02-09       | 11:30:00               | 12:00:00             | 2015-02-04 22:39:27 |
	+----+--------------+--------------------+---------------------------+-------------------------------+------------------+------------------------+----------------------+---------------------+

	mysql419 >>>>

### With Several Comments ###

	mysql419 >>SET FOREIGN_KEY_CHECKS=0;
	DROP TABLE IF EXISTS `appointment`;

	-- Create a table to hold the appointments
	CREATE TABLE appointment (
	`id` int NOT NULL AUTO_INCREMENT,
	`advisor_name` varchar(100),
	`student_name` varchar(100),--dkfjdkjgjg;;;;
	`advisor_email` varchar(100),
	`student_email` varchar(100),#ignore this;;;;
	`appointment_date` date,
	`appointment_start_time` time,
	`appointment_end_time` time,
	`date_created` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
	PRIMARY KEY (`id`)
	) ENGINE=InnoDB;

	-- Prefill Data
	INSERT appointment (advisor_name, student_name,
	advisor_email, student_email, appointment_date, appointment_start_time,
	appointment_end_time)
	VALUES ("Jeff Rix", "Rittie Chuaprasert",
	"rixj@onid.oregonstate.edu", "chuaprar@onid.oregonstate.edu",
	"2015-02-09", "11:30:00", "12:00:00");

	-- View initial Data
	SELECT * /* djkgjajgjfdgkjfadjgjhj */ F/*hello /*
	dkfkdlkgkgkhhk
	world*/ROM appointment;/*
	*/

	(query >> 'SET FOREIGN_KEY_CHECKS=0;')

	mysql419 >>(query >> 'DROP TABLE IF EXISTS `appointment`;')

	mysql419 >>>>>>>>>>>>>>>>>>>>>>>>>>>>(query >> 'CREATE TABLE appointment (`id` int NOT NULL AUTO_INCREMENT,`advisor_name` varchar(100),`student_name` varchar(100),`advisor_email` varchar(100),`student_email` varchar(100),`appointment_date` date,`appointment_start_time` time,`appointment_end_time` time,`date_created` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,PRIMARY KEY (`id`)) ENGINE=InnoDB;')

	mysql419 >>>>>>>>>>>>>>>>(query >> 'INSERT appointment (advisor_name, student_name,advisor_email, student_email, appointment_date, appointment_start_time,appointment_end_time)VALUES ("Jeff Rix", "Rittie Chuaprasert","rixj@onid.oregonstate.edu", "chuaprar@onid.oregonstate.edu","2015-02-09", "11:30:00", "12:00:00");')

	mysql419 >>>>>>(query >> 'SELECT *  FROM appointment;')
	+----+--------------+--------------------+---------------------------+-------------------------------+------------------+------------------------+----------------------+---------------------+
	| id | advisor_name | student_name       | advisor_email             | student_email                 | appointment_date | appointment_start_time | appointment_end_time | date_created        |
	+----+--------------+--------------------+---------------------------+-------------------------------+------------------+------------------------+----------------------+---------------------+
	|  1 | Jeff Rix     | Rittie Chuaprasert | rixj@onid.oregonstate.edu | chuaprar@onid.oregonstate.edu | 2015-02-09       | 11:30:00               | 12:00:00             | 2015-02-04 22:40:43 |
	+----+--------------+--------------------+---------------------------+-------------------------------+------------------+------------------------+----------------------+---------------------+

	mysql419 >>>>

