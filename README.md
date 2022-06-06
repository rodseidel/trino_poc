# Start
In the docker-compose.yaml folder, run command:

```bash 
docker-compose up
```

You should be able to access [Trino Web UI](http://localhost:8080).


You can access the Trino SQL Engine by creating into DBeaver a Trino Connection, with this values:
```
host: localhost
port: 8080
username: admin
no password

* Enable SSL=true into driver properties (not for now)
```
# Catalogs

The Redshift and EP catalogs are already configured.


# Users

**For while, although passwords are informed, this poc do not works correctly with authentication, only the authorization works fine.**

Users are created executing the command below in **/etc/trino/db** folder:

```bash
htpasswd -B -C 10 password.db user_name
```

Created users (user/password):
```
admin/admin
test1/123
test2/1234
test3/123
test4/1234
test5/123
test6/1234
test7/123
test8/1234
test9/123
```

# User Groups

User groups are created using file **/etc/trino/db/group.txt** as follows.

Created groups (group name:user list-comma separated):
```
poweruser:admin,test3
financial:test4
ecommerce:test5
dataanalyst:test9
superuser:test8
basicuser:test1
b2bdataanalyst:test2
```

# Access Control Rules

The rules are created in file **/etc/trino/db/rules.json**. 

Rules created (all rules allow only SELECT):
- group *poweruser*: access all tables from all schemas from all catalogs;
- group *superuser*: access all tables from all schemas from EP/Redshift catalogs only;
- group *ecommerce*: access all tables from all schemas from EP catalog only;
- group *financial*: access only tables _"wine_stg.mega_mgfin*"_ e _"wine_stg.mega_mgglo*"_ from Redshift catalog, but is not allowed query agn_st_cgc (CNPJ) from wine_stg.mega_mgglo_glo_agentes;
    - Queries that select disallowed columns will fail with error message:  Access Denied: Cannot select from table [catalog_name].[table_name].
- group *dataanalyst*: access only schema _wine_ds_ from Redshift catalog;
- group *basicreader*: access only only tables _"wine_stg.mega_mgglo*"_ but with masked sensible data.
    - Column agn_st_nome: first word concatenated with xxxxxxxx;
    - Column agn_st_cgc: string 'XX.XXX.XXX/' concatenated with last 7 chars.
- group *b2bdataanalyst*: access only table revenue_item from wine_ds schema from Redshift catalog, but only rows with bu_decription column equals 'B2B are allowed;
- user _test8_ can access only tables that contains *revenue* in table name;
- Users without group or not specificaly alowed: no access granted to all catalogs;


# In DBeaver or other Database Manager

Show configured catalogs:

```sql
show catalogs
```

Show schemas from catalog:
```sql
show schemas from [catalog_name]
```

Show tables from schemas:
```sql
show schemas from [catalog_name].[schema_name]
```

Show columns from table:
```sql
describe [catalog_name].[schema_name].[table_name]
```

You can query and join, like:
```sql
select a.col1, a.col2, a.col3, b.col1
from [catalog_nameA].[table_name] a
join [catalog_nameB].[table_name] b
  on a.id = b.fk_a_id
```




# Links 
- [Trino Catalog Connector](https://trino.io/docs/current/connector.html)
- [Password file documentation](https://trino.io/docs/current/security/password-file.html)
- [Secure internal communication with TSL](https://trino.io/docs/current/security/internal-communication.html)
- [File system access control](https://trino.io/docs/current/security/file-system-access-control.html)
- [Example rules.json file](https://github.com/operate-first/apps/blob/master/kfdefs/overlays/osc/osc-cl1/trino/configs/rules.json)
- [String operators](https://trino.io/docs/current/functions/string.html)

# Questions

- How to install apache2-utils package in container via Dockerfile?
