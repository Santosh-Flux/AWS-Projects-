# AWS-Projects- Deploy One Application server and create RDS MariaDB Database and access the RDS databases with MySQL Workbench Privately. 

**(A)  :- Deploy One Application server (Ubuntu)  & Create RDS MariaDB Database :-**
Go to the EC2 instance and we will create one Application server for that click on Launch instance. (Ubuntu) 
Give the name of the server and we will choose Ubuntu AMI as the Application server. The instance type will be free tier - t2.micro Select KeyPair. Select VPC >> We will deploy this server in the Public subnet so we will select Public Subnet and launch the instance. ![image](https://github.com/user-attachments/assets/71a35751-04fa-43b4-9da7-441bc7330f03)

Now go to the RDS service ![image](https://github.com/user-attachments/assets/3fa20f79-4675-4318-a168-b473aafa865e)
create a database by choosing MariaDB engine. ![image](https://github.com/user-attachments/assets/3c5d1445-3466-4f55-8d08-228f5fafc25f)
 Give manual username and password, Network type will be IPv4 and we will keep remaining things as default. We will NOT assign Public acccess so that Only Amazon EC2 instances & other resources inside the VPC can connect to our database. Click on create database and after that launch our Ubuntu Application server. 
 But we have not made the connectivity by whitelisting the port in security group of database. So, go to the VPC security group of RDS database and whitelist the Private IP address of Application server with Port-3306 (MySQL/Aurora). ![image](https://github.com/user-attachments/assets/d5935bcf-e92f-460f-8376-5a85d41f2c37)

 ![image](https://github.com/user-attachments/assets/7e25ae69-7c2c-4a84-85bc-e37c2ea680a6)
Also copy the Endpoint of database to use in command for the connectivity. 
On terminal, we will update server with command :- `#apt update` ![Screenshot 2024-10-16 235258](https://github.com/user-attachments/assets/6a1a7d8b-a157-43a1-aad2-fb0fc6172231)
Now we have to install the MySQL with command :- #apt install mysql-client-core-8.0 and then make connectivity with :-
#mysql -h mariadb-database.c34ce6ewi1tt.ap-south-1.rds.amazonaws.com -P 3306 -u admin –p
mariadb-database.c34ce6ewi1tt.ap-south-1.rds.amazonaws.com is the Endppoint of database and -P is Port and -p is for master username. 
![image](https://github.com/user-attachments/assets/cbc3337d-2bdf-4a53-807a-35ca5f51c5f0)
Now, we will create a database so use queries :- `show databases;`   >> `create database Cricket;`  It must show Query OK for the confirmation that our command is correct. 
We will create a table- Query :- `use Cricket;`  >>  `create table players(name varchar(20),team varchar(10),number varchar(30));`  >> `show tables; `  >> Now insert the players details as :- `insert into players values(“Virat”,”RCB”,”18”);`   >>  insert into players values(“Rohit”,”MI”,”47”);`  >>   `insert into players values(“Mahi”,”CSK”,”7”);`    >>   `select * from players;` ![image](https://github.com/user-attachments/assets/d5673057-cb38-453f-bfa5-aa94a87299e8)

**(B) Now we will access the RDS with MySQL Workbench.** 
Launch one Windows Application server in public subnet and whitelist its private IP in the security group of RDS database with the same port 3306 to make the connectivity. Launch that windows server and Download MySQL Workbench tool. (it may take more time to its better to download it on local machine and then copy-paste on windows application server) . After download, Install it and firstly to check connectivity is done or not, we will open "Windows Powershell" terminal and give command using endpoint :- `tnc mariadb-database.c34ce6ewi1tt.ap-south-1.rds.amazonaws.com -Port 3306`. The test results should be 'True'.
![image](https://github.com/user-attachments/assets/686d1809-7f2e-49d4-8612-4df7a2b9bac1)
Now open MySQL Workbench, click on '+' in front of MySQL connections >> Give connection name and hostname should be our Database Endpoint and username will be "admin" which we have given while creating RDS database. and click on Test connection. Enter the password which have already set-up while creating database. after click on 'OK', it will show as 'Successfully made the MySQL Connection'.
Click on test connection which we have just created, click on 'File' at the left corner >> 'New Query Tab' and then give Query as :- `show databases;` and click on Execute symbol. and it will show our database which we have created in Ubuntu Application server and we can edit it here as well and recheck it on ubuntu server. 
![image](https://github.com/user-attachments/assets/f5a0239c-3869-4b30-80ec-1c90e6bfc97d)

We are DONE~~ Wooo-hooo!! That means we are successfully able to access RDS from MySQL Workbench!! :) 
