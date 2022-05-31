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


# Create User

*For while, although passwords are informed, this poc do not works correctly with authentication, only the authorization works fine.*

In the ./trino/pass_db folder run to create the user _test_:

```bash
htpasswd -B -C 10 password.db test
```

Users created (user/password):
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

Groups created:
```
poweruser:admin,test3
financial:test4
ecommerce:test5
dataanalyst:test9
inactiveuser:test8
```

Rules created:
- *inactiveuser*: no access granted to all catalogs
- 


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