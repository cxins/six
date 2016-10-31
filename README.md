# six
<h1>RBAC权限管理</h1><br/>
<h2>1.查询用户test1可以查看的页面</h2><br/>
<h3>（1）SQL语句</h3><br/>
SELECT MenuName,MenuUrl <br/>
FROM sys_menu <br/>
where MenuID in<br/>
(SELECT PrivilegeAccessKey <br/>
FROM cf_privilege <br/>
where PrivilegeMasterKey in<br/>
    (SELECT RoleID <br/>
    FROM cf_role <br/>
    where RoleID in<br/><br/><br/>
        (SELECT RoleID <br/><br/>
        FROM cf_userrole <br/><br/>
        where UserID in<br/><br/>
            (SELECT UserID <br/><br/>
            FROM cf_user <br/><br/>
            where LoginName='test1')))<br/><br/>
and PrivilegeAccess='Sys_Menu');<br/><br/>
<h3>（2）伪代码</h3><br/><br/>
<1>输入用户名称test1，查询user表。<br/><br/>
<2>根据人员编号userID查询userrole表。<br/><br/>
<3>根据privilegemasterkey和privilegeaccess查询cf_privilege表。<br/><br/>
<4>获取privilegeID和menuname。<br/><br/>
<h3>（3）执行结果</h3><br/><br/>
<h2>2.查询用户test1对订单(order)页面中的操作权限</h2><br/><br/>
<h3>（1）SQL语句</h3><br/><br/>
SELECT * FROM sys_button<br/><br/>
where MenuNo=<br/><br/>
(select MenuNo from(<br/><br/>
    SELECT * <br/><br/>
    FROM sys_menu <br/><br/>
    where MenuID in<br/><br/>
        (SELECT PrivilegeAccessKey <br/>
        FROM cf_privilege <br/>
        where PrivilegeMasterKey in<br/>
            (SELECT RoleID <br/>
            FROM cf_role <br/>
            where RoleID in<br/>
                (SELECT RoleID <br/>
                FROM cf_userrole <br/>
                where UserID in<br/>
                    (SELECT UserID <br/>
                    FROM cf_user <br/>
                    where LoginName='test1')))<br/>
                and PrivilegeAccess='Sys_Menu')) temp<br/>
 where MenuName='订单');<br/>
<h3>（2）伪代码</h3><br/>
<1>输入用户名称test1，查询user表。<br/>
<2>根据人员编号userID查询userrole表。<br/>
<3>根据privilegemasterkey和privilegeaccess查询cf_privilege表。<br/>
<4>获取privilegeID和menuname。<br/>
<5>根据menuname和menuNo查询button表。<br/>
<6>获取按钮权限。<br/>

<h3>（3）执行结果</h3><br/>
