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
  --Must [conn /as sysdba]: grant select on APEX_190200.wwv_flow_request_verifications to <SCHEMA>;
  vAda varchar2(1):='N';
begin
  if length(:P9999_DCAPTCHA) <3 then
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

4. Open sqlplus, and connect as Sys DBA (user SYS), and then grant permission to user **schema** (example in my case is scott schema) :
```
sqlplus /nolog
conn /as sysdba
grant select on APEX_190200.wwv_flow_request_verifications to scott;
exit;
```

5. Done

Note:
for apex 19.2 uses APEX_190200, 19.1 APEX_190100, 18.1 APEX_180100, 18.0 APEX_180000, 5.1 APEX_050100, 5.0 APEX_050000.
