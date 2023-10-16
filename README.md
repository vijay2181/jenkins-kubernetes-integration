# jenkins-kubernetes-integration

JENKINS INSTALLATION LTS VERSION:
=================================
```
sudo apt update
```
```
sudo apt install fontconfig openjdk-17-jre
```
```
java -version
```
```
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
```
```
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```
```
sudo apt-get update
```
```
sudo apt-get install jenkins
```

```
ubuntu@ip-172-31-93-232:~$ java -version
openjdk version "17.0.8.1" 2023-08-24
OpenJDK Runtime Environment (build 17.0.8.1+1-Ubuntu-0ubuntu122.04)
```
```
ubuntu@ip-172-31-93-232:~$ jenkins --version
2.414.2
```
```
sudo systemctl enable jenkins
```
```
sudo systemctl start jenkins
```

![image](https://github.com/vijay2181/jenkins-kubernetes-integration/assets/66196388/46d275b9-d0ff-46d3-be33-b9aafb96e0b0)

![image](https://github.com/vijay2181/jenkins-kubernetes-integration/assets/66196388/768460cc-0da1-4c9c-80d2-64f7af5d4e27)

During the installation, the Jenkins installer creates an initial 32-character long alphanumeric
password. Use the following command to print the password on your terminal

```
ubuntu@ip-172-31-93-232:~$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```


![image](https://github.com/vijay2181/jenkins-kubernetes-integration/assets/66196388/62e57daa-3b5a-44cd-82f0-a7de08d2f8e9)


![image](https://github.com/vijay2181/jenkins-kubernetes-integration/assets/66196388/360d8366-61c1-45ac-8463-d4e30a5ddeab)










