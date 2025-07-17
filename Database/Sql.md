# 結構化查詢語言(SQL)

## SQL 四大指令分類
- **DDL（資料定義語言）**：CREATE, ALTER, DROP, TRUNCATE, RENAME, COMMENT
- **DML（資料操作語言）**：SELECT, INSERT, UPDATE, DELETE
- **DCL（資料控制語言）**：GRANT, REVOKE
- **TCL（交易控制語言）**：COMMIT, ROLLBACK, SAVEPOINT


## 視窗函數(Windows Function)

排名
---
### ROW_NUMBER()
- 練習：[LeetCode 511. Game Play Analysis I](https://leetcode.com/problems/game-play-analysis-i)

- 題目：要找出「每位玩家」，最早的登入時間

- 語法解釋：相同 player_id為一組，每個組別再依照 event_date由小到大排序，最後每個組別依序給予 row值
    - 每位玩家每日只會有一筆，所以使用 ROW_NUMBER()即可
```sql
ROW_NUMBER() OVER (PARTITION BY player_id ORDER BY event_date) as num
```

- 部分 sql執行結果：
```sql
SELECT player_id, 
       event_date, 
       ROW_NUMBER() OVER (PARTITION BY player_id ORDER BY event_date) as num
FROM Activity;
```

| PLAYER_ID | EVENT_DATE          | NUM |
| --------- | ------------------- | --- |
| 1         | 2016-03-01 00:00:00 | 1   |
| 1         | 2016-05-02 00:00:00 | 2   |
| 2         | 2017-06-25 00:00:00 | 1   |
| 3         | 2016-03-02 00:00:00 | 1   |
| 3         | 2018-07-03 00:00:00 | 2   |

- 範例答案：
```sql
SELECT player_id, 
       TO_CHAR(event_date, 'YYYY-MM-DD') as first_login 
FROM (
    SELECT player_id, 
           event_date, 
           ROW_NUMBER() OVER (PARTITION BY player_id ORDER BY event_date) as num
    FROM Activity
) WHERE num = 1;
```


#### RANK()
- 練習：[LeetCode 184. Department Highest Salary](https://leetcode.com/problems/department-highest-salary/)

- 題目：要找出「每個部門」，薪資最高的所有員工

- 語法解釋：相同 Department.id為一組，每個組別再依照 Employee.salary由大到小排序，最後每個組別依序給予 rank值，數值相同給予相同 rank，會跳號
    - 最高薪資員工可能有多個，所以使用 RANK()即可
```sql
RANK() OVER (PARTITION BY Department.id ORDER BY Employee.salary DESC) as rank 
```

- 部分 sql執行結果：
```sql
SELECT d.name, 
       e.name, 
       e.salary, 
       RANK() OVER (PARTITION BY d.id ORDER BY e.salary DESC) as rank 
FROM Department d 
JOIN Employee e ON d.id = e.departmentId;
```

| NAME  | NAME  | SALARY | RANK          |
| ----- | ----- | ------ | ------------- |
| IT    | Jim   | 90000  | 1             |
| IT    | Max   | 90000  | 1             |
| IT    | Joe   | 70000  | 3 這邊跳號了   | 
| Sales | Henry | 80000  | 1             |
| Sales | Sam   | 60000  | 2             |

- 範例答案：
```sql
SELECT sub.dName as Department, 
       sub.eName as Employee, 
       sub.eSalary as Salary 
FROM (
    SELECT d.name as dName, 
           e.name as eName, 
           e.salary as eSalary, 
           RANK() OVER (PARTITION BY d.id ORDER BY e.salary DESC) as rank 
    FROM Department d 
    JOIN Employee e ON d.id = e.departmentId
) sub
WHERE RANK = 1;
```


### DENSE_RANK()
- 練習：[LeetCode 185. Department Top Three Salaries](https://leetcode.com/problems/department-top-three-salaries/)

- 題目：要找出「每個部門」，最高 3個薪資 (top three unique salaries)的所有員工

- 語法解釋：相同 Department.id為一組，每個組別再依照 Employee.salary由大到小排序，最後每個組別依序給予 rank值，數值相同給予相同 rank，不跳號。
    - 因為需要取前 3個，相同薪資員工可能很多，所以需要採用不跳號的 DENSE_RANK()
```sql
DENSE_RANK() OVER (PARTITION BY Department.id ORDER BY Employee.salary DESC) as rank 
```

- 部分 sql執行結果：
```sql
SELECT d.name as dName, 
       e.name as eName, 
       e.salary as eSalary, 
       DENSE_RANK() OVER (PARTITION BY d.id ORDER BY e.salary DESC) as rank
FROM Department d 
JOIN Employee E ON d.id = e.departmentId;
```

| DNAME | ENAME | ESALARY | RANK            |
| ----- | ----- | ------- | --------------- |
| IT    | Max   | 90000   | 1               |
| IT    | Randy | 85000   | 2               |
| IT    | Joe   | 85000   | 2               |
| IT    | Will  | 70000   | 3 這邊沒有跳號   |
| IT    | Janet | 69000   | 4               |
| Sales | Henry | 80000   | 1               |
| Sales | Sam   | 60000   | 2               |

- 範例答案：
```sql
SELECT sub.dName as Department, 
       sub.eName as Employee, 
       sub.eSalary as Salary 
FROM (
    SELECT d.name as dName, 
           e.name as eName, 
           e.salary as eSalary, 
           DENSE_RANK() OVER (PARTITION BY d.id ORDER BY e.salary DESC) as rank
    FROM Department d 
    JOIN Employee E ON d.id = e.departmentId
) sub
WHERE rank <= 3;
```


聚合
---

前後比對
---

### LAG()

### LEAD()

首尾比對
---