---
title: leetcode之数据库练习笔记
categories: MySQL
tags: [MySQL]

---

**SQL Select语句完整的执行顺序：** 

```
(1) FROM <left_table>
(2) <join_type> JOIN <right_table>
(3) ON <join_condition>
(4) WHERE <where_condition>
(5) GROUP BY <group_by_list>
(6) WITH {CUBE | ROLLUP}
(7) HAVING <having_condition>
(8) SELECT
(9) DISTINCT
(9) ORDER BY <order_by_list>
(10) <TOP_specification> <select_list>
也就是
1、FROM：对FROM 子句中的前两个表执行笛卡尔积(交叉联接)，生成虚拟表VT1。
2、ON：对VT1 应用ON 筛选器，只有那些使为真才被插入到TV2。
3、OUTER (JOIN):如果指定了OUTER JOIN(相对于CROSS JOIN 或INNER JOIN)，保留表中未找到匹配的行将作为外部行添加到VT2，生成TV3。如果FROM 子句包含两个以上的表，则对上一个联接生成的结果表和下一个表重复执行步骤1 到步骤3，直到处理完所有的表位置。
4、WHERE：对TV3 应用WHERE 筛选器，只有使为true 的行才插入TV4。
5、GROUP BY：按GROUP BY 子句中的列列表对TV4 中的行进行分组，生成TV5。
6、CUTE|ROLLUP：把超组插入VT5，生成VT6。
7、HAVING：对VT6 应用HAVING 筛选器，只有使为true 的组插入到VT7。
8、SELECT：处理SELECT 列表，产生VT8。
9、DISTINCT：将重复的行从VT8 中删除，产品VT9。
10、ORDER BY：将VT9 中的行按ORDER BY 子句中的列列表顺序，生成一个游标(VC10)。
11、TOP：从VC10 的开始处选择指定数量或比例的行，生成表TV11，并返回给调用者。
```

**以上每个步骤都会产生一个虚拟表，该虚拟表被用作下一个步骤的输入。这些虚拟表对调用者(客户端应用程序或者外部查询)不可用。只有最后一步生成的表才会会给调用者。**

 [175. 组合两个表](https://leetcode-cn.com/problems/combine-two-tables)

```mysql
SELECT
    P.FirstName, P.LastName, A.City, A.State
FROM Person P
LEFT OUTER JOIN Address A
ON P.PersonId = A.PersonId
```

[176. 第二高的薪水](https://leetcode-cn.com/problems/second-highest-salary)

```mysql
SELECT 
(
    SELECT DISTINCT Salary
    FROM Employee ORDER BY Salary DESC
    LIMIT 1 OFFSET 1
)
SecondHighestSalary
```

[177. 第N高的薪水](https://leetcode-cn.com/problems/nth-highest-salary)

```mysql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
    set n = N-1;
  RETURN (
      # Write your MySQL query statement below.
      SELECT DISTINCT Salary
      FROM Employee ORDER BY Salary DESC
      LIMIT 1 OFFSET n
  );
END
```

[178. 分数排名](https://leetcode-cn.com/problems/rank-scores)

```mysql
# 对于 MySQL 解决方案，如果要转义用作列名的保留字，可以在关键字之前和之后使用撇号。例如 ‘Rank‘
select Score,
        dense_rank() over(order by Score desc) 'Rank'
from Scores
```

[180. 连续出现的数字](https://leetcode-cn.com/problems/consecutive-numbers)

```mysql
# 编写一个 SQL 查询，查找所有至少连续出现三次的数字
# Id-cast(row_number() over(partition by Num) as signed) 中间-是减号
select distinct num ConsecutiveNums
from
(
    select num, Id-cast(row_number() over(partition by Num) as signed) rnk
    from Logs
) t
group by num, rnk
having count(*) >= 3

# or
select distinct a.num ConsecutiveNums
from Logs a, Logs b, Logs c
where   a.Id = b.Id+1
    and b.Id = c.Id+1
    and a.Num = b.Num
    and b.Num = c.Num
```

[181. 超过经理收入的员工](https://leetcode-cn.com/problems/employees-earning-more-than-their-managers)

```mysql
# 把同一份表再次JOIN该表，条件是A.ManagerId = B.Id
SELECT A.Name Employee
FROM Employee A
LEFT OUTER JOIN Employee B
ON A.ManagerId = B.Id
WHERE A.Salary > B.Salary

# or
SELECT A.Name Employee
FROM Employee A
INNER JOIN Employee B
ON A.ManagerId = B.Id
WHERE A.Salary > B.Salary
```

[182. 查找重复的电子邮箱](https://leetcode-cn.com/problems/duplicate-emails)

```mysql
SELECT Email FROM Person GROUP BY Email HAVING COUNT(Email)>1;
```

[183. 从不订购的客户](https://leetcode-cn.com/problems/customers-who-never-order)

```mysql
# C.Name Customers重命名了  NOT IN 关键字
SELECT C.Name Customers
FROM Customers C
WHERE C.Id NOT IN 
(
    SELECT CustomerId FROM Orders
)
# or  ——这里左外连接了，驱动表有但是被驱动表没有的就可以筛选出来
SELECT C.Name Customers
FROM Customers C
LEFT OUTER JOIN Orders O
ON C.Id = O.CustomerId
WHERE O.CustomerId is null
```

[184. 部门工资最高的员工](https://leetcode-cn.com/problems/department-highest-salary)

```mysql
select d.Name Department, e.Name Employee, e.Salary
from Employee e, Department d
where e.DepartmentId = d.Id
    and (e.DepartmentId, Salary) in
        (select DepartmentId, max(Salary) Salary
            from Employee group by DepartmentId)
```

[185. 部门工资前三高的所有员工](https://leetcode-cn.com/problems/department-top-three-salaries)

```mysql
select Department, Employee, Salary
from
(
    select d.Name Department, e.Name Employee, Salary,
        dense_rank() over(partition by d.Name order by Salary desc) rnk
    from Employee e left join Department d
    on e.DepartmentId = d.Id
) t
where rnk <= 3 and Department is not null

# or 

```

[196. 删除重复的电子邮箱](https://leetcode-cn.com/problems/delete-duplicate-emails)

```mysql
DELETE p1 FROM Person p1, Person p2 
WHERE p1.Email = p2.Email AND p1.Id > p2.Id;

# or
DELETE FROM Person
WHERE Id NOT IN
(
    SELECT Id FROM
    (
        SELECT MIN(Id) Id FROM Person GROUP BY Email
    ) tempTableName
);

# or
select Department, Employee, Salary
from
(
    select d.Name Department, e.Name Employee, Salary,
        dense_rank() over(partition by d.Name order by Salary desc) rnk
    from Employee e right join Department d
    on e.DepartmentId = d.Id
) t
where rnk <= 3 and Employee is not null

# or 使用内连接，自动去除 null 的行
select Department, Employee, Salary
from
(
    select d.Name Department, e.Name Employee, Salary,
        dense_rank() over(partition by d.Name order by Salary desc) rnk
    from Employee e join Department d
    on e.DepartmentId = d.Id
) t
where rnk <= 3
```

[197. 上升的温度](https://leetcode-cn.com/problems/rising-temperature)

```mysql
select w1.Id
from Weather w1, Weather w2
where date_sub(w1.RecordDate, interval 1 day) = w2.RecordDate
        and w1.Temperature > w2.Temperature
        
# or
select w1.Id
from Weather w1, Weather w2
where datediff(w1.RecordDate, w2.RecordDate) = 1
        and w1.Temperature > w2.Temperature
      
# or
select w1.Id
from Weather w1, Weather w2
where date_add(w2.RecordDate, interval 1 day) = w1.RecordDate
        and w1.Temperature > w2.Temperature
```

[262. 行程和用户](https://leetcode-cn.com/problems/trips-and-users)

```mysql
select Request_at 'Day',
        round(avg(Status != 'completed'), 2) 'Cancellation Rate' # 必须加引号，有空格
from Trips t left join Users u
on t.Client_Id = u.Users_id
where Banned = 'No'
        and Request_at between '2013-10-01' and '2013-10-03'
group by Request_at  # 或者 group by Day， 不能写为 'Day'

# or
select Request_at 'Day',
        round(sum(Status != 'completed')/count(*), 2) 'Cancellation Rate' # 必须加引号，有空格
from Trips t left join Users u
on t.Client_Id = u.Users_id
where Banned = 'No'
        and Request_at between '2013-10-01' and '2013-10-03'
group by Request_at  # 或者 group by Day， 不能写为 'Day'
```

[511. 游戏玩法分析 I](https://leetcode-cn.com/problems/game-play-analysis-i)

```mysql
select player_id, min(event_date) first_login from Activity
group by player_id
```

[512. 游戏玩法分析 II](https://leetcode-cn.com/problems/game-play-analysis-ii)

```mysql
# 请编写一个 SQL 查询，描述每一个玩家首次登陆的设备名称
select a.player_id, a.device_id
from Activity a,
    (   select *, min(event_date) mindate
        from Activity
        group by player_id
    ) t
where a.player_id = t.player_id and a.event_date = t.mindate
```

[534. 游戏玩法分析 III](https://leetcode-cn.com/problems/game-play-analysis-iii)

```mysql
# 编写一个 SQL 查询，同时报告每组玩家和日期，以及玩家到目前为止玩了多少游戏。也就是说，在此日期之前玩家所玩的游戏总数。详细情况请查看示例。
select a1.player_id, a1.event_date, 
    sum(a2.games_played) games_played_so_far
from Activity a1, Activity a2
where a1.player_id = a2.player_id
    and a1.event_date >= a2.event_date
group by a1.player_id, a1.event_date
```

[550. 游戏玩法分析 IV](https://leetcode-cn.com/problems/game-play-analysis-iv)

```mysql
# 您需要计算从首次登录日期开始至少连续两天登录的玩家的数量，然后除以玩家总数。
select round(count(*)/(select count(distinct player_id) from Activity), 2) fraction
from Activity a
where (player_id, event_date) in
(
    select player_id, date_add(min(event_date), interval 1 day) first_day
    from Activity
    group by player_id
)
```

[569. 员工薪水中位数](https://leetcode-cn.com/problems/median-employee-salary)

```mysql
# 请编写SQL查询来查找每个公司的薪水中位数。
# 挑战点：你是否可以在不使用任何内置的SQL函数的情况下解决此问题。
select Id, Company, Salary
from
(
    select *, 
            row_number() over (partition by Company order by Salary) rnk,
            count(*) over (partition by Company) num
    from Employee
) t
where(
        (num%2=1 and rnk = floor(num/2)+1)
        or
        (num%2=0 and (rnk = floor(num/2) or rnk = floor(num/2)+1))
     )
```

[570. 至少有5名直接下属的经理](https://leetcode-cn.com/problems/managers-with-at-least-5-direct-reports)

[571. 给定数字的频率查询中位数](https://leetcode-cn.com/problems/find-median-given-frequency-of-numbers)

[574. 当选者](https://leetcode-cn.com/problems/winning-candidate)

[577. 员工奖金](https://leetcode-cn.com/problems/employee-bonus)

```mysql
select e.name, b.bonus 
from Employee e left join Bonus b
using (empId)
where b.bonus < 1000 or b.bonus is null
```

[578. 查询回答率最高的问题](https://leetcode-cn.com/problems/get-highest-answer-rate-question)

[579. 查询员工的累计薪水](https://leetcode-cn.com/problems/find-cumulative-salary-of-an-employee)

[580. 统计各专业学生人数](https://leetcode-cn.com/problems/count-student-number-in-departments)

[584. 寻找用户推荐人](https://leetcode-cn.com/problems/find-customer-referee)

```mysql
# Write your MySQL query statement below
select name from customer
where referee_id not in (2) or referee_id is null
# or
select name from customer
where referee_id != 2 or referee_id is null
# or
select name from customer
where referee_id <> 2 or referee_id is null
```

[585. 2016年的投资](https://leetcode-cn.com/problems/investments-in-2016)

[586. 订单最多的客户](https://leetcode-cn.com/problems/customer-placing-the-largest-number-of-orders)

```mysql
select t.customer_number customer_number from
(
    select customer_number, count(customer_number) amount from orders
    group by customer_number
    order by amount desc
    limit 1 offset 0
) t
# or
select customer_number from orders
group by customer_number
order by count(customer_number) desc
limit 1
```

[595. 大的国家](https://leetcode-cn.com/problems/big-countries)

```mysql
# Write your MySQL query statement below
SELECT name, population, area 
FROM World 
WHERE population > 25000000 
OR area > 3000000;
```

[596. 超过5名学生的课](https://leetcode-cn.com/problems/classes-more-than-5-students)

```mysql
# 列出所有超过或等于5名学生的课
SELECT class FROM courses GROUP BY class
HAVING COUNT(DISTINCT student) >= 5;
```

[597. 好友申请 I：总体通过率](https://leetcode-cn.com/problems/friend-requests-i-overall-acceptance-rate)



[601. 体育馆的人流量](https://leetcode-cn.com/problems/human-traffic-of-stadium)



[602. 好友申请 II ：谁有最多的好友](https://leetcode-cn.com/problems/friend-requests-ii-who-has-the-most-friends)



[603. 连续空余座位](https://leetcode-cn.com/problems/consecutive-available-seats)

```mysql
select distinct a.seat_id
    from cinema a join cinema b
    on abs(a.seat_id-b.seat_id) = 1 and a.free = true and b.free = true
order by a.seat_id
```

[607. 销售员](https://leetcode-cn.com/problems/sales-person)

```mysql
# 输出所有表 salesperson 中，没有向公司 ‘RED’ 销售任何东西的销售员
select name from salesperson
where sales_id not in
(
    select sales_id from
    orders left join company
    using(com_id)
    where company.name = 'RED'
)
```

[608. 树节点](https://leetcode-cn.com/problems/tree-node)

[610. 判断三角形](https://leetcode-cn.com/problems/triangle-judgement)

```mysql
select x, y, z, 
    case when x+y > z and x+z > y and y+z > x then 'Yes'
        else 'No'
    end as triangle
from triangle
# or
select *, if(x+y>z and x+z>y and y+z>x, 'Yes', 'No') as triangle
from triangle
```

[612. 平面上的最近距离](https://leetcode-cn.com/problems/shortest-distance-in-a-plane)

[613. 直线上的最近距离](https://leetcode-cn.com/problems/shortest-distance-in-a-line)

```mysql
select min(abs(p1.x - p2.x)) shortest 
from point p1 left join point p2
on p1.x != p2.x
```

[614. 二级关注者](https://leetcode-cn.com/problems/second-degree-follower)

[615. 平均工资：部门与公司比较](https://leetcode-cn.com/problems/average-salary-departments-vs-company)

[618. 学生地理信息报告](https://leetcode-cn.com/problems/students-report-by-geography)

[619. 只出现一次的最大数字](https://leetcode-cn.com/problems/biggest-single-number)

[620. 有趣的电影](https://leetcode-cn.com/problems/not-boring-movies)

```mysql
SELECT * FROM cinema 
WHERE description != 'boring' 
        AND id%2 = 1 
ORDER BY rating DESC

# 或者用 mod(id,2) = 1
# !=也可以用<>
```

[626. 换座位](https://leetcode-cn.com/problems/exchange-seats)

[627. 变更性别](https://leetcode-cn.com/problems/swap-salary)

```mysql
UPDATE salary SET sex=
    CASE sex 
        WHEN 'm' THEN 'f'
        ELSE 'm'
    END;
    
# or
update salary set sex = IF(sex = 'm', 'f', 'm')

# or 
update salary set sex = char(ASCII(sex)^ASCII('m')^ASCII('f'))
```

[1045. 买下所有产品的客户](https://leetcode-cn.com/problems/customers-who-bought-all-products)



[1050. 合作过至少三次的演员和导演](https://leetcode-cn.com/problems/actors-and-directors-who-cooperated-at-least-three-times)

```mysql
select actor_id, director_id from ActorDirector
group by actor_id, director_id
having count(*) >= 3
```

[1068. 产品销售分析 I](https://leetcode-cn.com/problems/product-sales-analysis-i)

```mysql
# 写一条SQL 查询语句获取产品表 Product 中所有的 产品名称 product name 以及 该产品在 Sales 表中相对应的 上市年份 year 和 价格 price。
select b.product_name, a.year, a.price
from Sales a
join Product b
on a.product_id = b.product_id
```

[1069. 产品销售分析 II](https://leetcode-cn.com/problems/product-sales-analysis-ii)

```mysql
select s.product_id, sum(s.quantity) total_quantity from
    Sales s left join Product p
    using(product_id)
    group by s.product_id
#   order by s.product_id # 可选排序
```

[1070. 产品销售分析 III](https://leetcode-cn.com/problems/product-sales-analysis-iii)

[1075. 项目员工 I](https://leetcode-cn.com/problems/project-employees-i)

[1076. 项目员工II](https://leetcode-cn.com/problems/project-employees-ii)

[1077. 项目员工 III](https://leetcode-cn.com/problems/project-employees-iii)

[1082. 销售分析 I](https://leetcode-cn.com/problems/sales-analysis-i)

```mysql
# 这样写是因为总销售额最高的销售者有可能不止一位
select seller_id from Sales
group by seller_id
having sum(price) = 
        (
            select sum(price) as totalincome from Sales
            group by seller_id
            order by totalincome desc
            limit 1
        )
        
# or ——all 函数，所有的都要满足
select seller_id from Sales
group by seller_id
having sum(price) >=
        all(
            select sum(price) from Sales
            group by seller_id
        )
```

[1083. 销售分析 II](https://leetcode-cn.com/problems/sales-analysis-ii)



[1084. 销售分析III](https://leetcode-cn.com/problems/sales-analysis-iii)

[1097. 游戏玩法分析 V](https://leetcode-cn.com/problems/game-play-analysis-v)

[1098. 小众书籍](https://leetcode-cn.com/problems/unpopular-books)

[1107. 每日新用户统计](https://leetcode-cn.com/problems/new-users-daily-count)

[1112. 每位学生的最高成绩](https://leetcode-cn.com/problems/highest-grade-for-each-student)

[1113. 报告的记录](https://leetcode-cn.com/problems/reported-posts)

[1126. 查询活跃业务](https://leetcode-cn.com/problems/active-businesses)

[1127. 用户购买平台](https://leetcode-cn.com/problems/user-purchase-platform)

[1132. 报告的记录 II](https://leetcode-cn.com/problems/reported-posts-ii)

[1141. 查询近30天活跃用户数](https://leetcode-cn.com/problems/user-activity-for-the-past-30-days-i)

[1142. 过去30天的用户活动 II](https://leetcode-cn.com/problems/user-activity-for-the-past-30-days-ii)

[1148. 文章浏览 I](https://leetcode-cn.com/problems/article-views-i)

```mysql
select distinct author_id as id from Views
where viewer_id = author_id
order by id
```

[1149. 文章浏览 II](https://leetcode-cn.com/problems/article-views-ii)

[1158. 市场分析 I](https://leetcode-cn.com/problems/market-analysis-i)

[1159. 市场分析 II](https://leetcode-cn.com/problems/market-analysis-ii)

[1164. 指定日期的产品价格](https://leetcode-cn.com/problems/product-price-at-a-given-date)

[1173. 即时食物配送 I](https://leetcode-cn.com/problems/immediate-food-delivery-i)

[1174. 即时食物配送 II](https://leetcode-cn.com/problems/immediate-food-delivery-ii)

[1179. 重新格式化部门表](https://leetcode-cn.com/problems/reformat-department-table)

[1193. 每月交易 I](https://leetcode-cn.com/problems/monthly-transactions-i)

[1194. 锦标赛优胜者](https://leetcode-cn.com/problems/tournament-winners)

[1204. 最后一个能进入电梯的人](https://leetcode-cn.com/problems/last-person-to-fit-in-the-bus)

[1205. 每月交易II](https://leetcode-cn.com/problems/monthly-transactions-ii)

[1211. 查询结果的质量和占比](https://leetcode-cn.com/problems/queries-quality-and-percentage)

[1212. 查询球队积分](https://leetcode-cn.com/problems/team-scores-in-football-tournament)

[1225. 报告系统状态的连续日期](https://leetcode-cn.com/problems/report-contiguous-dates)

[1241. 每个帖子的评论数](https://leetcode-cn.com/problems/number-of-comments-per-post)

[1251. 平均售价](https://leetcode-cn.com/problems/average-selling-price)

```mysql
select p.product_id, round(sum(p.price*s.units)/sum(s.units), 2) average_price
from Prices p left join UnitsSold s
on p.product_id = s.product_id 
    and s.purchase_date >= p.start_date
    and s.purchase_date <= p.end_date
group by p.product_id

# Result table:
+------------+---------------+
| product_id | average_price |
+------------+---------------+
| 1          | 6.96          |
| 2          | 16.96         |
+------------+---------------+
平均售价 = 产品总价 / 销售的产品数量。
产品 1 的平均售价 = ((100 * 5)+(15 * 20) )/ 115 = 6.96
产品 2 的平均售价 = ((200 * 15)+(30 * 30) )/ 230 = 16.96
```

[1264. 页面推荐](https://leetcode-cn.com/problems/page-recommendations)

[1270. 向公司CEO汇报工作的所有人](https://leetcode-cn.com/problems/all-people-report-to-the-given-manager)

[1280. 学生们参加各科测试的次数](https://leetcode-cn.com/problems/students-and-examinations)

[1285. 找到连续区间的开始和结束数字](https://leetcode-cn.com/problems/find-the-start-and-end-number-of-continuous-ranges)

[1294. 不同国家的天气类型](https://leetcode-cn.com/problems/weather-type-in-each-country)

```mysql
select country_name, 
        case when avg(weather_state)<= 15 then 'Cold'
             when avg(weather_state)>= 25 then 'Hot'
             else 'Warm' end as weather_type
from Countries left join Weather
using (country_id)
# where day between '2019-11-01' and '2019-11-30'
where day like '2019-11%' # 也可以
group by country_id
```

[1303. 求团队人数](https://leetcode-cn.com/problems/find-the-team-size)

```mysql
# 自连接，按着 team_id，有重复的 team_id，产生了笛卡尔积
select a.employee_id, count(*) team_size from 
Employee a left join Employee b
on a.team_id = b.team_id # 产生了笛卡尔积
group by a.employee_id

# or 
select a.employee_id, temp.team_size from 
Employee a left join
(
    select team_id, count(*) team_size from Employee
    group by team_id
) temp
# on a.team_id = temp.team_id 也可以用 using
using(team_id)
```

[1308. 不同性别每日分数总计](https://leetcode-cn.com/problems/running-total-for-different-genders)

[1321. 餐馆营业额变化增长](https://leetcode-cn.com/problems/restaurant-growth)

[1322. 广告效果](https://leetcode-cn.com/problems/ads-performance)

[1327. 列出指定时间段内所有的下单产品](https://leetcode-cn.com/problems/list-the-products-ordered-in-a-period)

```mysql
select product_name, unit from
(
    select product_id, sum(unit) unit from Orders
    where(order_date >= '2020-02-01' 
            and order_date < '2020-03-01')
    group by product_id
) t left join Products
using(product_id)
where unit >= 100
```

[1336. 每次访问的交易次数](https://leetcode-cn.com/problems/number-of-transactions-per-visit)

[1341. 电影评分](https://leetcode-cn.com/problems/movie-rating)

[1350. 院系无效的学生](https://leetcode-cn.com/problems/students-with-invalid-departments)

```mysql
select a.id, a.name
from Students a
where a.department_id not in (
    select distinct id from Departments
)
# or
select a.id, a.name
from Students a left join Departments b
on b.id = a.department_id
where b.id is null # 笛卡尔积，不存在的院系id会为null
```

[1355. 活动参与者](https://leetcode-cn.com/problems/activity-participants)

[1364. 顾客的可信联系人数量](https://leetcode-cn.com/problems/number-of-trusted-contacts-of-a-customer)

[1369. 获取最近第二次的活动](https://leetcode-cn.com/problems/get-the-second-most-recent-activity)

[1378. 使用唯一标识码替换员工ID](https://leetcode-cn.com/problems/replace-employee-id-with-the-unique-identifier)

```mysql
select a.unique_id, b.name from
Employees b
left join EmployeeUNI a
on a.id = b.id
```

[1384. 按年度列出销售总额](https://leetcode-cn.com/problems/total-sales-amount-by-year)

[1393. 股票的资本损益](https://leetcode-cn.com/problems/capital-gainloss)

[1398. 购买了产品 A 和产品 B 却没有购买产品 C 的顾客](https://leetcode-cn.com/problems/customers-who-bought-products-a-and-b-but-not-c)

[1407. 排名靠前的旅行者](https://leetcode-cn.com/problems/top-travellers)

```mysql
#写一段 SQL , 报告每个用户的旅行距离.
#返回的结果表单, 以 travelled_distance 降序排列,
#如果有两个或者更多的用户旅行了相同的距离, 那么再以 name 升序排列.

select name, ifnull(dis,0) travelled_distance from
(
    select user_id id, sum(distance) dis
    from Rides
    group by user_id
) t right join Users
using(id)
order by travelled_distance desc, name

# or
select name, ifnull(sum(distance),0) travelled_distance from
Users left join Rides
on Users.id = Rides.user_id
group by Users.id
order by travelled_distance desc, name
```

[1412. 查找成绩处于中游的学生](https://leetcode-cn.com/problems/find-the-quiet-students-in-all-exams)

[1421. 净现值查询](https://leetcode-cn.com/problems/npv-queries)

[1435. 制作会话柱状图](https://leetcode-cn.com/problems/create-a-session-bar-chart)

[1440. 计算布尔表达式的值](https://leetcode-cn.com/problems/evaluate-boolean-expression)

[1445. 苹果和桔子](https://leetcode-cn.com/problems/apples-oranges)

[1454. 活跃用户](https://leetcode-cn.com/problems/active-users)

![image-20211206201332507](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211206201340.png)

```mysql
# 参考MYSQL实现排名函数RANK，DENSE_RANK和ROW_NUMBER
# 使用over子句中的排序语句对记录进行排序，然后按照这个顺序生成序号
with table1 as
(
    select id, login_date, 
            dense_rank() over(partition by id order by login_date) rnk
    from Logins
) # 建立表名
select distinct table1.id, name
from table1 left join Accounts a
on table1.id = a.id
group by id, date_sub(login_date, interval rnk day)
having count(distinct login_date) >= 5
order by id
```

[1459. 矩形面积](https://leetcode-cn.com/problems/rectangles-area)

[1468. 计算税后工资](https://leetcode-cn.com/problems/calculate-salaries)

[1479. 周内每天的销售情况](https://leetcode-cn.com/problems/sales-by-day-of-the-week)

[1485. 按日期分组销售产品](https://leetcode-cn.com/problems/group-sold-products-by-the-date)

group_concat()：group by 产生的同一个分组中的值连接起来，返回一个字符串结果。

- 语法：`group_concat( [distinct] 要连接的字段 [order by 排序字段 asc/desc ] [separator '分隔符'] )`

```mysql
# 编写一个 SQL 查询来查找每个日期、销售的不同产品的数量及其名称
select sell_date, count(distinct product) num_sold, 
    group_concat(distinct product order by product separator ',') products from Activities
group by sell_date
order by sell_date
```

[1495. 上月播放的儿童适宜电影](https://leetcode-cn.com/problems/friendly-movies-streamed-last-month)

[1501. 可以放心投资的国家](https://leetcode-cn.com/problems/countries-you-can-safely-invest-in)

[1511. 消费者下单频率](https://leetcode-cn.com/problems/customer-order-frequency)

[1517. 查找拥有有效邮箱的用户](https://leetcode-cn.com/problems/find-users-with-valid-e-mails)

[1527. 患某种疾病的患者](https://leetcode-cn.com/problems/patients-with-a-condition)

[1532. 最近的三笔订单](https://leetcode-cn.com/problems/the-most-recent-three-orders)

[1543. 产品名称格式修复](https://leetcode-cn.com/problems/fix-product-name-format)

[1549. 每件商品的最新订单](https://leetcode-cn.com/problems/the-most-recent-orders-for-each-product)

[1555. 银行账户概要](https://leetcode-cn.com/problems/bank-account-summary)

[1565. 按月统计订单数与顾客数](https://leetcode-cn.com/problems/unique-orders-and-customers-per-month)

[1571. 仓库经理](https://leetcode-cn.com/problems/warehouse-manager)

[1581. 进店却未进行过交易的顾客](https://leetcode-cn.com/problems/customer-who-visited-but-did-not-make-any-transactions)

[1587. 银行账户概要 II](https://leetcode-cn.com/problems/bank-account-summary-ii)

[1596. 每位顾客最经常订购的商品](https://leetcode-cn.com/problems/the-most-frequently-ordered-products-for-each-customer)

[1607. 没有卖出的卖家](https://leetcode-cn.com/problems/sellers-with-no-sales)

[1613. 找到遗失的ID](https://leetcode-cn.com/problems/find-the-missing-ids)

[1623. 三人国家代表队](https://leetcode-cn.com/problems/all-valid-triplets-that-can-represent-a-country)

[1633. 各赛事的用户注册率](https://leetcode-cn.com/problems/percentage-of-users-attended-a-contest)

[1635. Hopper 公司查询 I](https://leetcode-cn.com/problems/hopper-company-queries-i)

[1645. Hopper Company Queries II](https://leetcode-cn.com/problems/hopper-company-queries-ii)

[1651. Hopper Company Queries III](https://leetcode-cn.com/problems/hopper-company-queries-iii)

[1661. 每台机器的进程平均运行时间](https://leetcode-cn.com/problems/average-time-of-process-per-machine)

[1667. 修复表中的名字](https://leetcode-cn.com/problems/fix-names-in-a-table)

[1677. 发票中的产品金额](https://leetcode-cn.com/problems/products-worth-over-invoices)

[1683. 无效的推文](https://leetcode-cn.com/problems/invalid-tweets)

[1693. 每天的领导和合伙人](https://leetcode-cn.com/problems/daily-leads-and-partners)

[1699. 两人之间的通话次数](https://leetcode-cn.com/problems/number-of-calls-between-two-persons)

[1709. 访问日期之间最大的空档期](https://leetcode-cn.com/problems/biggest-window-between-visits)

[1715. 苹果和橘子的个数](https://leetcode-cn.com/problems/count-apples-and-oranges)

[1729. 求关注者的数量](https://leetcode-cn.com/problems/find-followers-count)

[1731. 每位经理的下属员工数量](https://leetcode-cn.com/problems/the-number-of-employees-which-report-to-each-employee)

```mysql
select 
round((sum(order_date = customer_pref_delivery_date)/count(*)*100), 2)
immediate_percentage from Delivery

# 或者avg 函数返回占比
select 
round((avg(order_date = customer_pref_delivery_date)*100), 2)
immediate_percentage from Delivery
```

[1741. 查找每个员工花费的总时间](https://leetcode-cn.com/problems/find-total-time-spent-by-each-employee)

[1747. 应该被禁止的Leetflex账户](https://leetcode-cn.com/problems/leetflex-banned-accounts)

[1757. 可回收且低脂的产品](https://leetcode-cn.com/problems/recyclable-and-low-fat-products)

[1767. 寻找没有被执行的任务对](https://leetcode-cn.com/problems/find-the-subtasks-that-did-not-execute)

[1777. 每家商店的产品价格](https://leetcode-cn.com/problems/products-price-for-each-store)

[1783. 大满贯数量](https://leetcode-cn.com/problems/grand-slam-titles)

[1789. 员工的直属部门](https://leetcode-cn.com/problems/primary-department-for-each-employee)

[1795. 每个产品在不同商店的价格](https://leetcode-cn.com/problems/rearrange-products-table)

[1809. 没有广告的剧集](https://leetcode-cn.com/problems/ad-free-sessions)

[1811. 寻找面试候选人](https://leetcode-cn.com/problems/find-interview-candidates)

[1821. 寻找今年具有正收入的客户](https://leetcode-cn.com/problems/find-customers-with-positive-revenue-this-year)

[1831. 每天的最大交易](https://leetcode-cn.com/problems/maximum-transaction-each-day)

[1841. 联赛信息统计](https://leetcode-cn.com/problems/league-statistics)

[1843. 可疑银行账户](https://leetcode-cn.com/problems/suspicious-bank-accounts)

[1853. 转换日期格式](https://leetcode-cn.com/problems/convert-date-format)

[1867. Orders With Maximum Quantity Above Average](https://leetcode-cn.com/problems/orders-with-maximum-quantity-above-average)

[1873. 计算特殊奖金](https://leetcode-cn.com/problems/calculate-special-bonus)

[1875. 将工资相同的雇员分组](https://leetcode-cn.com/problems/group-employees-of-the-same-salary)

[1890. 2020年最后一次登录](https://leetcode-cn.com/problems/the-latest-login-in-2020)

[1892. 页面推荐Ⅱ](https://leetcode-cn.com/problems/page-recommendations-ii)

[1907. 按分类统计薪水](https://leetcode-cn.com/problems/count-salary-categories)

[1917. Leetcodify Friends Recommendations](https://leetcode-cn.com/problems/leetcodify-friends-recommendations)

[1919. 兴趣相同的朋友](https://leetcode-cn.com/problems/leetcodify-similar-friends)

[1934. Confirmation Rate](https://leetcode-cn.com/problems/confirmation-rate)

[1939. Users That Actively Request Confirmation Messages](https://leetcode-cn.com/problems/users-that-actively-request-confirmation-messages)

[1949. Strong Friendship](https://leetcode-cn.com/problems/strong-friendship)

[1951. 查询具有最多共同关注者的所有两两结对组](https://leetcode-cn.com/problems/all-the-pairs-with-the-maximum-number-of-common-followers)

[1965. 丢失信息的雇员](https://leetcode-cn.com/problems/employees-with-missing-information)

[1972. First and Last Call On the Same Day](https://leetcode-cn.com/problems/first-and-last-call-on-the-same-day)

[1978. Employees Whose Manager Left the Company](https://leetcode-cn.com/problems/employees-whose-manager-left-the-company)

[1988. Find Cutoff Score for Each School](https://leetcode-cn.com/problems/find-cutoff-score-for-each-school)

[1990. 统计实验的数量](https://leetcode-cn.com/problems/count-the-number-of-experiments)

[2004. The Number of Seniors and Juniors to Join the Company](https://leetcode-cn.com/problems/the-number-of-seniors-and-juniors-to-join-the-company)

[2010. The Number of Seniors and Juniors to Join the Company II](https://leetcode-cn.com/problems/the-number-of-seniors-and-juniors-to-join-the-company-ii)

[2020. Number of Accounts That Did Not Stream](https://leetcode-cn.com/problems/number-of-accounts-that-did-not-stream)

[2026. Low-Quality Problems](https://leetcode-cn.com/problems/low-quality-problems)

[2041. 面试中被录取的候选人](https://leetcode-cn.com/problems/accepted-candidates-from-the-interviews)

[2051. The Category of Each Member in the Store](https://leetcode-cn.com/problems/the-category-of-each-member-in-the-store)

[2066. Account Balance](https://leetcode-cn.com/problems/account-balance)

[2072. The Winner University](https://leetcode-cn.com/problems/the-winner-university)

[2082. 富有客户的数量](https://leetcode-cn.com/problems/the-number-of-rich-customers)

[2084. Drop Type 1 Orders for Customers With Type 0 Orders](https://leetcode-cn.com/problems/drop-type-1-orders-for-customers-with-type-0-orders)

