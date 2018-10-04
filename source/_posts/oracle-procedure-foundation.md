---
title: Oracle 存储过程基础
date: 2018-03-31
categories: rookie
tags:
- oracle
- SQL
---

开发环境：PL/SQL Developer.
- 查看 Oracle 版本相关信息：`select * from v$version`;

## 创建存储过程

### 标准语法

```sql
CREATE [OR REPLACE] PROCEDURE procedure_name
[(parameter_name [IN | OUT | IN OUT] <type> [, ...])]
{IS | AS}
    [declare variable]
BEGIN
    <procedure_body>
    [EXCEPTION exception handlers]
END procedure_name;
```

- procedure_name: 存储过程名称。
- [OR REPLACE] 为可选语法。表示如果已经存在该存储过程，则进行修改和覆盖。
- parameter_name: 参数。参数是可选的，它由参数名，参数类型和数据类型组成。
    - 参数类型有三种，分别为 `IN, OUT, IN OUT`。
        - IN 表示输入参数，可以理解为一个只读常量，它是按引用传递的，同时它也是默认的参数类型；
        - OUT 表示输出参数，它是按值传递的变量，作为输出结果；
        - IN OUT 即可作输入参数，也可作输出参数，它也是按值传递的变量。
    - 对于参数的数据类型，注意不能指定数据类型的具体长度或精度，比如 VARCHAR2(10) 是不被允许的，只能用 VARCHAR2。此外，还可以使用 **DEFAULT** 关键字给参数声明默认值（仅支持 IN 类型的参数）。例如：`parameter_name IN VARCHAR2 DEFAULT 'default_value'`.
- {IS | AS}: 二选一。该关键字开始到 BEGIN 之间的 [declare variable] 是可选的，可用于声明存储过程中要用到的变量。
- procedure_body: 存储过程的逻辑执行部分。
- [EXCEPTION exception handlers] 为异常处理块，可选。

### 完整语法低清无码图

![procedure syntax](https://i.loli.net/2018/10/04/5bb4ef4d1162b.png)

## Hello World

```sql
CREATE OR REPLACE PROCEDURE hello
AS
BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello World!'); -- 打印 Hello World!
END hello;
```

## 一些基本操作

使用 `:=` 对变量进行赋值。

```sql
variable := 'value';
```

使用 `||` 拼接字符串。

```sql
you := 'na' || 'i' || 've';
```

执行 sql 语句，使用关键字 `into` 将结果赋值给变量。

```sql
SELECT COUNT(1) into variable FROM TABLE WHERE column1 = param1;
SELECT column1, column2 into variable1, variable2 FROM TABLE; -- 给多个变量赋值
```

使用 `execute immediate` 执行动态 sql 语句，并用 `using` 进行**顺序**传参。

```sql
dynamic_column := 'column1';
dynamic_sql := 'UPDATE TABLE SET ' || dynamic_column || ' = :1 WHERE column2 = :2 ';
EXECUTE IMMEDIATE dynamic_sql USING param1, param2;
```

在以上这段动态 sql 语句中，参数 param1 和 param2 将会按照顺序依次放到 `:1` 和 `:2` 的位置上。

## 条件判断

### IF

```sql
IF <condition1> THEN
    <statements to execute when condition1 is true>
ELSIF <condition2> THEN
    <statements to execute when condition2 is true>
ELSE
    <statements to execute when both condition1 and condition2 are false>
END IF;
```

- 注意不要漏掉 `THEN` 关键字。
- 可以有多个 ELSIF，但最多有一个 ELSE。
- 当 IF 和所有的 ELSIF 的条件都是 false 时，才执行 ELSE 块。
- ELSIF 和 ELSE 都是可选的。

### CASE

```sql
CASE <selector>
    WHEN <expression1> THEN <statements to execute>
    WHEN <expression2> THEN <statements to execute>
    ...
    ELSE <statements to execute>
END CASE;
```

如果 selector 的值和某个 WHEN 块的 expression 相等，则执行该块，否则，将执行 ELSE 块。ELSE 是可选的，如果不声明一个 ELSE 块，则默认带上一个隐式的 ELSE 块 `ELSE RAISE CASE_NOT_FOUND;`，意思是如果没有匹配到任何一个 WHEN 块，则抛出 `CASE_NOT_FOUND` 的异常。举个栗子：

```sql
grade := 'D';
CASE grade
    WHEN 'A' THEN DBMS_OUTPUT.PUT_LINE('Perfect');
    WHEN 'B' THEN DBMS_OUTPUT.PUT_LINE('Good');
    WHEN 'C' THEN DBMS_OUTPUT.PUT_LINE('Poor');
    ELSE DBMS_OUTPUT.PUT_LINE('No such grade');
END CASE;
```

显然，打印的结果为 No such grade。

## 循环

### loop 循环

```sql
LOOP
    <statements to execute>
    [EXIT [WHEN condition];]
END LOOP;
```

`[EXIT [WHEN condition];]` 是可选的；若不带上 EXIT 则相当于一个死循环，所以一般使用 `EXIT` 配合 `WHEN` 条件来跳出整个循环，如下：

```sql
LOOP
    variable := variable + 1;
    EXIT WHEN variable > 10; -- 当 variable 的值大于 10 时，跳出循环
END LOOP;
```

### while 循环

```sql
WHILE <condition>
LOOP
    <statements to execute>
END LOOP;
```

如果条件为 true，则执行 loop 块里的逻辑；如果条件为 false，则退出整个循环；如果条件一开始就是 false，则循环体一次也不会执行。

### for 循环

```sql
FOR <loop_counter> IN [REVERSE] <lowest_number>..<highest_number>
LOOP
    <statements to execute>
END LOOP;
```

需要注意的是 `REVERSE` 关键字，它是可选的，如果声明为 REVERSE，则循环将反向计数！举个栗子：

```sql
FOR i IN REVERSE 1..5
LOOP
    result := i * 2;
    DBMS_OUTPUT.PUT_LINE(result);
END LOOP;
```

由于是反向计数，所以打印出来的结果依次为：10, 8, 6, 4, 2。

### 使用 cursor 游标循环

```sql
FOR <record_index> in <cursor_name>
LOOP
    <statements to execute>
END LOOP;
```

举个栗子：

```sql
CREATE OR REPLACE PROCEDURE test_cursor_loop
AS
    CURSOR test_c IS SELECT column1, column2 FROM TABLE;
BEGIN
    FOR test_record IN test_c
    LOOP
        DBMS_OUTPUT.PUT_LINE(test_record.column1 || test_record.column2);
    END LOOP;
END test_cursor_loop;
```

在这个栗子中，我们首先定义了一个游标 test_c，从表中查出了包含 column1 和 column2 字段的所有数据放到该游标中，然后循环。test_record 为每条记录，通过 test_record.column_name 获取到具体的字段。当游标中所有的数据都被取出来后，则结束该循环。

## 异常处理

- 一些常见的系统异常：
> 
> NO_DATA_FOUND: 没有查询到数据。
> TOO_MANY_ROWS: 预期拿到一条数据，却返回了多条数据。
> ZERO_DIVIDE: 除数为 0。
- 自定义异常：声明 `exception_name EXCEPTION;`。
- 使用关键字 `RAISE` 抛出一个自定义异常： `RAISE exception_name;`。
- 捕获异常并处理：`EXCEPTION WHEN exception_name THEN <exception handlers>`。

举个栗子：

```sql
CREATE OR REPLACE PROCEDURE test_exception (param IN NUMBER)
AS
    test_e EXCEPTION;
BEGIN
    IF param = 0 THEN
        DBMS_OUTPUT.PUT_LINE('No exception!');
    ELSE
        RAISE test_e;
    END IF;

    EXCEPTION
        WHEN test_e THEN DBMS_OUTPUT.PUT_LINE('Throw test_e exception!');
        WHEN OTHERS THEN DBMS_OUTPUT.PUT_LINE('Throw other exception!');
END test_exception;
```

在这个栗子中，我们自定义了一个名叫 test_e 的异常，并在逻辑代码里说明当参数 param 不等于 0 时，则抛出该异常，最后在异常处理块中进行捕获和处理。注意，异常处理块中的 `OTHERS` 代表的是除了 test_e 异常外的所有异常。

## 执行存储过程

### 普通调用

```sql
BEGIN
    procedure_name(param1, param2...);
END;
```

无参数的存储过程可以使用括号也可以不使用，即 procedure_name; 或 procedure_name();

### 指定参数调用

```sql
DECLARE
    V1 VARCHAR2(2) := 'v1';
    V2 VARCHAR2(2) := 'v2';
    V3 NUMBER(1);
BEGIN
    procedure_name(param1 => V1, param2 => V2, param3 => V3);
END;
```

## 删除存储过程

```sql
DROP PROCEDURE procedure_name;
```

## Reference

- [CREATE PROCEDURE](https://docs.oracle.com/cd/B19306_01/server.102/b14200/statements_6009.htm)
- [PL/SQL - Procedures](https://www.tutorialspoint.com/plsql/plsql_procedures.htm)
- [Oracle / PLSQL: Procedures](https://www.techonthenet.com/oracle/procedures.php)
- [PL/SQL Language Fundamentals](https://docs.oracle.com/database/121/LNPLS/fundamentals.htm#LNPLS002)
