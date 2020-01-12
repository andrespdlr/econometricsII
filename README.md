# Econometría II

<p>Este repositorio contiene 3 directorios</p>

| directorios  |
| ------------ |
| books  	 |
| lectures  	 |
| databases  	 |


## Requirements

- Nodejs and NPM
- Python >= 3.5 (Anaconda environment is recommended)
- PostgreSQL database engine
- Twitter account to get access tokens
- New python environment: `conda create --name myenv`


**Python requirements**

- `pip install -r requirements.txt`


After installing these packages, install scikit-learn. Scikit-learn version must be 0.19.1:

- `pip install scikit-learn==0.19.1` or `conda install -c anaconda scikit-learn==0.19.1`


## Possible changes in site-packages


*Anaconda3\envs\myenv\Lib\site-packages\sklearn\model_selection\_search.py*

```
from collections import Mapping, namedtuple, defaultdict, Sequence
```

change to

```
from collections.abc import Mapping, Sequence
from collections import namedtuple, defaultdict
```


*Anaconda3\envs\myenv\Lib\site-packages\sklearn\model_selection\_split.py*

```
from collections import Iterable
```

change to

```
from collections.abc import Iterable
```


*Anaconda3\envs\myenv\Lib\site-packages\sklearn\utils\multiclass.py*

```
from collections import Sequence
```

change to

```
from collections.abc import Sequence
```


*Anaconda3\envs\myenv\Lib\site-packages\sklearn\utils\__init__.py*

C:\Anaconda3\envs\twitter_api_search\

```
from collections import Sequence
```

change to

```
from collections.abc import Sequence
```



## Configuration

#### Step 1
- Create a PostgreSQL Database Engine
- Download software: https://www.postgresql.org/
- Local:
	- Run `createdb -U postgres mydb`
		- Run `psql -U postgres mydb` to verify db was \
		created correctly.
			- List databases: `\l` (*mydb* must appear in table)
			- Exit: `\q`

	- Create tables:
		- Run in *sql* directory: \
		`psql -U postgres -d mydb -a -f create_tables.sql`
		- Run `psql -U postgres mydb` to verify tables were \
		created correctly.
			- List tables in database: `\dt`
			<br />

			| Schema |     Name     	| Type  |  Owner   |
			| ------ | -------------- | ----- | -------- |
			| public | hashtags       | table | postgres |
			| public | media          | table | postgres |
			| public | media_tweet    | table | postgres |
			| public | mentions       | table | postgres |
			| public | place          | table | postgres |
			| public | query          | table | postgres |
			| public | search_keyword | table | postgres |
			| public | search_profile | table | postgres |
			| public | tweet          | table | postgres |
			| public | urls           | table | postgres |
			| public | users          | table | postgres |

			- Exit: `\q`

- Remote (i.e. *AWS*):
	- Create an AWS account
	- Launch a RDS Service using a PostgreSQL Database
	- Resource: https://aws.amazon.com/es/rds/
	- In order to create tables use software that allows remote connections \
	such as PGAdmin and use data in *./sql/create_db_instance.sql*

#### Step 2
- Run `pip install -U -r requirements.txt` (Using Anaconda Environment)
- It will install **python** dependencies.

#### Step 3
- Run `npm i --production` in *dashboard* directory
- It will install **nodejs** dependencies and create *node_modules* directory

#### Step 4
- Run `npm i nodemon -g`
- It will install a command that will help later easily to launch *dashboard*

#### Step 5
- Run `python config.py`
- This command will create 5 files in *config* directory:
	- twitter_auth.json
		- twitter credentials
	- db.json
	- db.js
		- database authentication
	- pythonPath.js
		- python programming route
	- timezone.json
		- config timezone to convert tweet dates into your timezone
		- Timezone examples:
			- America/New_York
			- America/Bogota
			- America/Mexico_City
			- More: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones

#### Step 6
- Run `python add_queries.py "./data/queries.csv"` to add queries into database
- *"./data/queries.csv"* file contains queries.
	- field **query** (search term)
	- field **lang** (language)
	- field **res_type** (type of tweet: 'mixed' or 'popular')



---

### DATA EXAMPLE `--->` queries & profiles

---

##### profiles.csv

- `Syntax: 1 = True; 0 = False`

<br />

| screen_name |  exclude_replies | 	include_rts  | timezone 					 | context								 |
| ----------- | -----------------| ------------- | ------------------- | ----------------------- |
| user 				| 1				      	 | 0		 				 | America/New_York		 | description or context	 |
| user 				| 0				      	 | 1		 				 | America/Bogota			 | description or context	 |
| user 				| 1				      	 | 1		 				 | America/Mexico_City | description or context	 |


##### queries.csv

<br />

| query				|  lang   | res_type  | timezone 					  | context								 	 |
| -----------	| ------- | ----------| ------------------- | ------------------------ |
| keyword			| es			| mixed			| America/New_York	  | description or context	 |
| keyword			| en			| recent		| America/Bogota		  | description or context	 |
| keyword			| pt			| popular		| America/Mexico_City | description or context	 |


---

#### add_data.py `--->` queries & profiles

---

`python add_data.py --help`

<br />

`python add_data.py --data queries.csv --database mydb`

-	A csv file is required.
- Arguments are required:
	- `-d` or `--data` points out to data file
	- `--database` points out to postgreSQL database


---

### Issues


**NPM Install Babel**

`npm install --save-dev "babel-core@^7.0.0-bridge.0"`

<br />

- get minutes

` (0.06 / 100 * 60)`

---

### Backup database

`pg_dump -h localhost -p 5432 -U postgres mydb > D:\backup.sql`

---
