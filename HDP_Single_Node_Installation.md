HDP Single Node Installation Steps

1. Choose an instance type - m4.xlarge

  m4.xlarge            4              13           16           EBS Only              $0.215 per Hour

2. Choose storage type - EBS

  Root - 8 GB (GP2 SSD Provisioned)
  EBS - 100 GB (GP2 SSD Provisioned)

3. Create a new security group. Access to all ports from any IP address

4. Launch Instance. Made use of existing Key Pair - Syntelli_Instances_Key

5. Public IP generated for EC2 Instance - 54.242.32.246
Update your instance

6. Connected to EC2 instance using Putty.
Installed "httpd" Apache service for verifying if the IP address of the EC2 instance is accessible to any machine, trying to access it.

To start service: $ sudo systemctl start httpd.service
to stop the service: $ sudo systemctl stop httpd.service

7. Add additional volume to EC2 instance. Followed steps from HDP Installation on AWS

8. Disable "selinux"

9. Install firewall service. Stop the service.

sudo systemctl disable firewalld
sudo service firewalld stop

10. Install NTP service for your instance. Check to see, if NTP is enabled.

11. Modify Umask value. Changed it from 002 to 022.

12. Create SSH Keygen. Reboot the instance.

13. Copy the "id_rsa.pub" to "authorized_keys". SSH into your instance using the internal IP provided from the AWS EC2 Console.

14. Modify the hosts file.

Entry for present EC2 instance:
  10.0.1.210      ip-10-0-1-210.ec2.internal      hdpsinglenode.syntelli.com

15. Ambari Repo

  "wget -nv http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.4.2.0/ambari.repo -O /etc/yum.repos.d/ambari.repo"

16. Disable IP tables before installing Ambari Server on your Master Node. Refer the HDP installation document for more details.

  RHEL/CentOS/Oracle Linux 7
  sudo systemctl disable firewalld
  sudo service firewalld stop

17. I followed the steps, I had followed earlier in HDP AWS Installation document.

18. Start Amabri Server.

17. Access the Ambari Web UI with external IP of the AWS EC2 instance that was just created. Where 8080 is the port to access the Ambari Web UI.

  example: http://54.166.201.121:8080

  Login with credentials:
  username: admin
  password: admin

18. Click on "Launch Install Wizard" to create a new cluster.

  Cluster name: hdpsinglenode

19. Installing services:

  "ntpd" service was disabled. I had to re-enable it. Refer HDP AWS Installation document for solving this problem.

20. Once all checks are complete, choose services.

21. Configure services

  1. Hive
    dbname:     hive
    dbusername: hive
    dbpassword: hive

  2. Oozie
    dbname: oozie
    dbusername: oozie
    dbpassword: oozie

  3. Accumulo
    instancename: hdp-accumulo-instance
    accumulorootpassword: admin
    instancesecret: accumulo

  4. Ambari metrics
    grafanaadmin: admin
    grafanaadminpassword: grafana

  5. Knox
    knoxhostgateway: ip-10-0-1-210.ec2.internal
    knoxmastersecret: knox

  6. Smart Sense
    user: admin
    password: smart

21. Modify Hbase root directory, if needed. Review all the services to be installed, before deploying.
