

```powershell

# Connect
redis-cli -h hostname -p port
# If Redis requires authentication
redis-cli -a password

# Get the version to search for exploits
INFO # The information and statistics about the Redis server

# This shows ONLY databases that contain keys
INFO keyspace

# Redis database indices are numbered, not named. You can discover which databases contain data using these methods:
SELECT database_index_number # e.g. Select 0

# List all keys (use carefully in production)
KEYS *

# Pattern matching
KEYS user:*
KEYS session:*
KEYS *pattern*


# Number of keys in current database
DBSIZE

# View Key Values
GET mykey

# Set a key value
SET key value

```