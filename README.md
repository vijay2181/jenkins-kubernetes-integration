# jenkins-kubernetes-integration

# overview
![image](https://github.com/vijay2181/jenkins-kubernetes-integration/assets/66196388/88392723-dc1b-4421-8092-d1d495688c4e)


1.Jenkins Installation LTS Version:
-----------------------------------
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



2.Setting Up Docker in Jenkins Server:
--------------------------------------

```
# Install Latest Docker
 curl -fsSL https://get.docker.com -o get-docker.sh
 sudo sh get-docker.sh
```

ubuntu@ip-172-31-23-222:~$ docker --version

Docker version 20.10.21, build 20.10.21-0ubuntu1~22.04.3

```
sudo systemctl enable docker 
sudo systemctl start docker
```

ubuntu@ip-172-31-23-222:~$ docker ps

Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/json": dial unix /var/run/docker.sock: connect: permission denied

```
sudo usermod -aG docker <current_username>
sudo usermod -aG docker jenkins
```

still if you do **docker ps** you will get permission error, so you need to refreh the terminal or exit from terminal and login back

Note that the changes will take effect the next time the user logs in or opens a new terminal session.

To reflect the group membership changes in the same terminal session.run below commands

```
sudo su - jenkins
newgrp docker
docker ps 
```

3.Setup Kubernetes Cluster:
---------------------------
- follow below link to setup k8s cluster using kubeadm

```
https://github.com/vijay2181/kubernetes-setup-using-kubeadm.git
```


4.Setup Jenkins Server to deploy applications into Kubernetes Cluster:
----------------------------------------------------------------------
We can deploy docker applications into Kubernetes cluster from Jenkins using two approaches.

1) Using Kubernetes Continues Deploy Plugin
• Go to Jenkins > Manage Plugins > Available > Search for Kubernetes Continues Deploy > Select And Install.

• Add kube config information in Jenkins Credentials.

Jenkins > Credentials > Add Credentials > Select Kind As Kubernates Configuration ( Kubeconfig) > Select enter directly radio button > copy kubeconfig content from Kubenertes cluster

• Use KubernetesDeploy in pipeline script

```
stage("Deploy To Kuberates Cluster"){
   kubernetesDeploy(
     configs: 'springBootMongo.yml',
     kubeconfigId: 'KUBERNATES_CONFIG',
     enableConfigSubstitution: true
     )
}
```






