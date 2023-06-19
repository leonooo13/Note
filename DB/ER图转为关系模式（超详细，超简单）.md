# ER图转为关系模式（超详细，超简单）

相关系列：[ER 图转为关系模式](https://blog.csdn.net/qq_43179428/article/details/105309972 "ER 图转为关系模式")[无损分解和保持依赖](https://blog.csdn.net/qq_43179428/article/details/105706351 "无损分解和保持依赖")[3NF 分解与 BCNF 分解](https://blog.csdn.net/qq_43179428/article/details/105596526 "3NF 分解与 BCNF 分解")[正则覆盖与候选码](https://blog.csdn.net/qq_43179428/article/details/105563296 "正则覆盖与候选码")[如何设计 ER 图（弱实体集）](https://blog.csdn.net/qq_43179428/article/details/105309432 "如何设计 ER 图（弱实体集）")[如何设计 ER 图（映射基数）](https://blog.csdn.net/qq_43179428/article/details/105307911 "如何设计 ER 图（映射基数）")

### 目录

-   [1. 简单属性的强实体集](#1_12 "1. 简单属性的强实体集")
-   [2. 派生属性不出现](#2_17 "2. 派生属性不出现")
-   [3. 复合属性由子属性代替](#3_23 "3. 复合属性由子属性代替")
-   [3. 多值属性也构建](#3_29 "3. 多值属性也构建")
-   [4. 弱实体集](#4_45 "4. 弱实体集")
-   [5. 联系集](#5_53 "5. 联系集")
-   [去掉冗余](#_66 "去掉冗余")
-   [ER 图转为关系模式的例子](#ER_85 "ER 图转为关系模式的例子")
-   \* \*

# 1. 简单属性的强实体集

![](https://img-blog.csdnimg.cn/20200404142219815.png)

-   人（**身份证号**，姓名，性别）
-   \* \*

# 2. 派生属性不出现

![](https://img-blog.csdnimg.cn/20200404142333704.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTc5NDI4,size_16,color_FFFFFF,t_70)

由身份证号可以推算出年龄，所以年龄是派生属性。

-   人（**身份证号**，姓名）
-   \* \*

# 3. 复合属性由子属性代替

![](https://img-blog.csdnimg.cn/20200404143033490.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTc5NDI4,size_16,color_FFFFFF,t_70)

-   人（**身份证号**，姓，名）
-   \* \*

# 3. 多值属性也构建

![](https://img-blog.csdnimg.cn/20200404142549242.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTc5NDI4,size_16,color_FFFFFF,t_70)

对于一个多值属性 M, 构建关系模式 R，该模式包含一个对应于 M 的属性 A, 以及对应于 M 所在的实体集或联系集的主码的属性。R 的主码由 R 的所有属性构成。

-   人（**身份证号**，姓名）；
-   人 - 电话（**身份证号**，**电话号码**）
-   \* \*

如果一个实体集只有一个主码和一个多值属性，我们只转换为一个关系模式。 &#x20;

![](https://img-blog.csdnimg.cn/20200404144357280.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTc5NDI4,size_16,color_FFFFFF,t_70)

-   人（**身份证号，电话号码**）
-   \* \*

# 4. 弱实体集

**弱实体集的主码**由自身的**分辨符**和所依赖的**强实体集的主码**构成。**其属性**由自身的属性和所依赖的强实体集的主码构成。

![](https://img-blog.csdnimg.cn/20200809121737285.png)

-   section（**course\_id,sec\_id,semester,year**）(所列属性均为主码)
-   \* \*

# 5. 联系集

**属性**由自身属性和所有参与此联系的所有实体集的主码构成。**联系集的主码：**&#x20;

1.  二元 &#x20;
    1.  一对一：两个实体集中的主码任意选择一个 &#x20;
    2.  **一对多**或者**多对一**：选择多的那一方 &#x20;
    3.  多对多：两个实体集的主码的并集
2.  n 元 &#x20;
    1.  对于边上没有箭头的 n 元联系集：所有参与实体集的主码属性构成的并集 &#x20;
    2.  边上有一个箭头的 n 元联系集：不在箭头侧的实体集的主码属性构成的并集

-   \* \*

# 去掉冗余

如果我们将所有的联系集全部写出来，那么有的会存在冗余。 &#x20;
消除冗余

1.  一般情况下，连接弱实体集与其所依赖的强实体集的联系集的模式是冗余的，而且在基于 E-R 图的关系数据库设计中不必给出。**即标识性联系是冗余的**
2.  多对一的联系集 AB 可以和全部参与的一方 A 合并。那么 A 将包含 A 和 AB 属性的并集。

![](https://img-blog.csdnimg.cn/20200809122107738.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTc5NDI4,size_16,color_FFFFFF,t_70)

```
inst_dept可以和instructor合并，最终是instructor(ID,dept_name,name,salary)
主键：ID
外键：dept_name
这样就消去了联系集inst_dept

```

. &#x20;

1.  在一对一的联系的情况下，联系集的关系模式可以跟参与联系的任何一个实体集的模式进行合并。

-   \* \*

# ER 图转为关系模式的例子

![](https://img-blog.csdnimg.cn/20200509125029366.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTc5NDI4,size_16,color_FFFFFF,t_70)

```java
classroom(building, room_number, capacity)

主键：building, room_number

 

department(dept_name, building, budget)

主键：dept_name

 

course(course_id, title, dept_name, credits)

主键：course_id

外键：dept_name

 

instructor( ID , name, dept_name, salary)

主键：ID

外键：dept_name

 

section(course_id, sec_id, semester, year, building, room_number, time_slot_id)

主键：course_id, sec_id, semester, year

外键1：course_id

外键2：building, room_number
section是弱实体集，依赖强实体集course。这样确定了主键
同时又参与两个 一对多关系模型（classroom,time_slot）
 

teaches( ID , course_id, sec_id, semester, year)

主键：ID , course_id, sec_id, semester, year

外键1：ID

外键2：course_id, sec_id, semester, year

 

student( ID , name, dept_name, tot_cred)

主键：ID

外键：dept_name

 

takes( ID , course_id, sec_id, semester, year, grade)

主键：ID , course_id, sec_id, semester, year

外键1：ID

外键2：course_id, sec_id, semester, year

 

advisor(s_ID , i_ID )

主键：s_ID

外键1：s_ID

外键2：i_ID

 

time_slot(time_slot_id, day, start_time, end_time)

主键：time_slot_id, day, start_time

 

prereq(course_id, prereq_id)

主键：course_id, prereq_id

外键1：course_id

外键2：prereq_id

 

```

参考：数据库系统概念第六版&#x20;

[https://blog.csdn.net/qq\_43179428/article/details/105309972](https://blog.csdn.net/qq_43179428/article/details/105309972 "https://blog.csdn.net/qq_43179428/article/details/105309972")
