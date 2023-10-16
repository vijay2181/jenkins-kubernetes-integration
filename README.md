# jenkins-kubernetes-integration

JENKINS INSTALLATION LTS VERSION:
=================================
```
sudo apt update
sudo apt install fontconfig openjdk-17-jre
java -version
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins
```

ubuntu@ip-172-31-93-232:~$ java -version
openjdk version "17.0.8.1" 2023-08-24
OpenJDK Runtime Environment (build 17.0.8.1+1-Ubuntu-0ubuntu122.04)


ubuntu@ip-172-31-93-232:~$ jenkins --version
2.414.2

```
sudo systemctl enable jenkins
sudo systemctl start jenkins
```
