# Overtime

A simple program to register overtime work hours.

## Dependencies
- Sqlite3
- Bash >= 4.2


## Installation
1. Clone this repo.
2. Change directory to the repo and make the program executable with

```bash
chmod +x overtime
```

4. (Optional) add to path to access it anywhere

```bash
export PATH=$PATH:`pwd`
```

5. Run the initial installation. This step will create a sqlite database named `overtime` (you may specify another name with the `-c` flag, in which case this has to be provided in all commands) with one table called `overtime`. This is where all logged hours will be stored.

```bash
overtime install
```

6. You're good to go. A good starting point is probably

```bash
overtime -h
```

## Examples

Add two and a half hour overtime for today with a custom message:

```bash
overtime add -m "Worked a lot today" 2 30
# or, if you used a custom db name (this goes for all examples below as well):
overtime -c my_custom_db_name add -m "Worked a lot today" 2 30
```

Add one hour on another day:

```bash
overtime add -d "2022-02-02" 1
```

Add 30 minutes of negative hours. There are three ways to achieve this. Either use the `flex` subcommand, the `add` subcommand with the `-n` flag or the `add` subcommand with negative numbers:

```bash
overtime flex 0 30
# or:
overtime add -n 0 30
# or:
overtime add 0 -30
```

See the log

```bash
overtime log
+----+---------------------+------------+-------+---------+--------------------+
| id |     created_at      |    date    | hours | minutes |      message       |
+----+---------------------+------------+-------+---------+--------------------+
| 3  | 2024-12-16 10:35:12 | 2024-12-16 | 0     | -30     |                    |
| 2  | 2024-12-16 10:35:06 | 2024-12-16 | 1     | 0       |                    |
| 1  | 2024-12-16 10:34:57 | 2024-12-16 | 2     | 30      | Worked a lot today |
+----+---------------------+------------+-------+---------+--------------------+
```

Check your current balance

```bash
overtime balance
Current balance: 3 hours and 30.0 minutes
```

Undo the last entry

```bash
overtime undo
overtime log
+----+---------------------+------------+-------+---------+--------------------+
| id |     created_at      |    date    | hours | minutes |      message       |
+----+---------------------+------------+-------+---------+--------------------+
| 2  | 2024-12-16 10:35:06 | 2024-12-16 | 1     | 0       |                    |
| 1  | 2024-12-16 10:34:57 | 2024-12-16 | 2     | 30      | Worked a lot today |
+----+---------------------+------------+-------+---------+--------------------+
```

You can of course also work with the data directly from sqlite:

```bash
sqlite3 overtime.db

sqlite> SELECT * FROM overtime;
1|2024-12-16 10:34:57|2024-12-16|2|30|Worked a lot today
2|2024-12-16 10:35:06|2024-12-16|1|0|
```
