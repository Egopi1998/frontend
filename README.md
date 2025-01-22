required plugins : 

Pipeline stage view  --> Enhance the visibility.
AnsiColor
Pipeline Utility Steps --> for reading json file / Zip the files
Nexus Artifact Uploader
Rebuild --> Enables users to rebuild a previous Jenkins job with the same parameters, making reruns quick and efficient.

CI -- uplaoding artifact to Nexus

Nexus :
------
ubuntu OS user name is ubuntu
we must give a key-pair for this to login
wait for 5min to complete setup

public-ip:8081 --> to access 
sign in
default user name --> admin 

Configure AWS cred in Jenkins agent to connect with AWS for frontend-deploy job

Maven type artifact:
-----------------------
to identify easly 
group id --> com.expense
artifact id --> backend 
1.0.0 --> version 

http://public_ip:8081/repository/backend/  session 45 (from 20 mins)

CI - up steam job
CD - down stream job






