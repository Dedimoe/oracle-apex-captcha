# oracle-apex-captcha
oracle-apex-captcha

On page 9999 or 101 

1. Add page item : P9999_DCAPTCHA

2. On P9999_DCAPTCHA **Post Text** item property, put this code:

```
<div style="background-color:#FFF; border:1px solid #ececec; padding:3px;">
<img width="25" height="25",=""src="APEX_190200.wwv_flow_image_generator.get_image?p_position=1&p_sessionid=&APP_SESSION.">
<img width="25" height="25",=""src="APEX_190200.wwv_flow_image_generator.get_image?p_position=2&p_sessionid=&APP_SESSION.">
<img width="25" height="25",=""src="APEX_190200.wwv_flow_image_generator.get_image?p_position=3&p_sessionid=&APP_SESSION.">
<img width="25" height="25",=""src="APEX_190200.wwv_flow_image_generator.get_image?p_position=4&p_sessionid=&APP_SESSION.">
<img width="25" height="25",=""src="APEX_190200.wwv_flow_image_generator.get_image?p_position=5&p_sessionid=&APP_SESSION.">
</div>
```
3. On **Processing Tab** create process with new name : DCaptcha with PL/SQL code :

```
declare
  --Copyright (C) 2019 Dedi Mulyadi <dedi@dr.com>
  --Harus [conn /as sysdba]: grant select on APEX_190200.wwv_flow_request_verifications to <SCHEMA>;
  --                         grant select on APEX_190200.wwv_flow_request_verifications to blockchain_wks;
  vAda varchar2(1):='N';
begin
  if trim (:P9999_DCAPTCHA) is null then
    raise_application_error(-20001, 
    'Silahkan konfirmasi kode DCaptcha.');
  end if;
  begin
    select 'Y' into vAda
    from APEX_190200.wwv_flow_request_verifications
    where session_id = :APP_SESSION
      and substr(VERIFICATION_STRING,1,5) = :P9999_DCAPTCHA;
  exception
    when others then
    raise_application_error(-20001,
    'Silahkan konfirmasi kode DCaptcha.');
  end;
  if vAda = 'Y' then 
    null;
  else
    raise_application_error(-20001,
    'Silahkan konfirmasi kode DCaptcha.');
  end if;
end;
```

4. Open plsql, and connect as Sys DBA (user SYS), and then grant permission to user **schema** (example in my case is scott schema) :
```
sqlplus /nolog
conn /as sysdba
grant select on APEX_190200.wwv_flow_request_verifications to scott;
exit;
```

5. Done
