title: PL/SQL CASE
categories:
  - oracle
date: 2012-08-15 17:46:44
---

1.CASE中存在selector，不返回值

```sql

declare input_str varchar2(20):=('&str');
        begin
                case input_str
                     when 'A' then dbms_output.put_line(input_str||1);
                     when 'B' then dbms_output.put_line(input_str||2);
                     when 'C' then dbms_output.put_line(input_str||3);
                     when 'D' then dbms_output.put_line(input_str||4);
                     when 'E' then dbms_output.put_line(input_str||5);
                     else dbms_output.put_line('输入未知字符!');
                end case;
        end;

```

2.CASE中存在selector，作为表达式使用

```sql
declare input_str varchar2(20):=('&str');result_srt varchar2(50);
        begin
                result_srt:=case input_str
                     when 'A' then input_str||1
                     when 'B' then input_str||2
                     when 'C' then input_str||3
                     when 'D' then input_str||4
                     when 'E' then input_str||5
                     else '输入未知字符!'
                end;
                dbms_output.put_line(result_srt);
        end;
```

3.不使用CASE中的选择器搜索CASE

```sql
declare input_str varchar2(20):=('&str');result_srt varchar2(50);
        begin
                result_srt:=case
                     when input_str='A' then input_str||1
                     when input_str='B' then input_str||2
                     when input_str='C' then input_str||3
                     when input_str='D' then input_str||4
                     when input_str='E' then input_str||5
                     else '输入未知字符!'
                end;
                dbms_output.put_line(result_srt);
        end;
```
