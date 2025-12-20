
```powershell
# installation
sudo apt update && sudo apt install psql

# Connection:
psql -U christine -h localhost -p 1234 # -p  is port

# list existing databases
\list
\l 

# connect/use a database
\c <db_name>
\connection  <db_name>

# Display Tables
\dt

# Query a table
SELECT * FROM <TableName>;

```