# HDP 2.5 Single Node Installation Steps

1. Choose an instance type - m4.xlarge

        Instance Type: m4.xlarge            
        CPU Cores: 4
        RAM: 16
        Storage: EBS Only              
        Instance Cost: $0.215 per Hour

2. Choose storage type - EBS

        Root - 8 GB (GP2 SSD Provisioned)
        EBS - 100 GB (GP2 SSD Provisioned)

3. Create a new security group. Access to all ports from any IP address

4. Launch Instance. I made use of existing Key Pair, that I had already created.

5. Connected to EC2 instance using Putty.

  Installed "httpd" Apache service for verifying if the IP address of the EC2 instance is accessible to any machine, trying to access it.

  To start the Apache service:

        $ sudo systemctl start httpd.service

  To stop the Apache service:

        $ sudo systemctl stop httpd.service

6. Add additional volume to EC2 instance. Followed steps from HDP Installation on AWS.

7. Disable "selinux". Refer HDP Installation on AWS document.

8. Install firewall service. Stop the service.

        $ sudo systemctl disable firewalld
        $ sudo service firewalld stop

9. Install NTP service for your instance. Check to see, if NTP is enabled.

10. Modify Umask value. Changed it from 002 to 022.

11. Create SSH Keygen and then Reboot the instance.

12. Copy the "id_rsa.pub" to "authorized_keys". SSH into your instance using the internal IP provided from the AWS EC2 Console.

13. Modify the hosts file.

        #Entry for present EC2 instance:
        10.0.1.210      ip-10-0-1-210.ec2.internal      hdpsinglenode.syntelli.com

14. Ambari Repo

        "wget -nv http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.4.2.0/ambari.repo -O /etc/yum.repos.d/ambari.repo"

15. Disable IP tables before installing Ambari Server on your Master Node. Refer the HDP installation document for more details.

  On RHEL/CentOS/Oracle Linux 7

        $ sudo systemctl disable firewalld
        $ sudo service firewalld stop

16. I followed the steps, which I had followed earlier in HDP on AWS Installation document.

17. Start Amabri Server.

18. Access the Ambari Web UI with external IP of the AWS EC2 instance that was just created. Where 8080 is the port to access the Ambari Web UI.

        example: http://54.166.201.121:8080

  Login with credentials:

        username: admin
        password: admin

19. Click on "Launch Install Wizard" to create a new cluster.

        Cluster name: hdpsinglenode

20. Installing services:

  "ntpd" service was disabled. I had to re-enable it. Refer HDP AWS Installation document for solving this problem.

21. Once all checks are complete, choose services.

22. Configure services

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

23. Modify Hbase root directory, if needed. Review all the services to be installed, before deploying.
