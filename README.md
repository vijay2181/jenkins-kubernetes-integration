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
```
sudo systemctl restart jenkins
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


2) Install kubectl and add kubeconfig in Jenkins server

Install Kubectl in Jenkins Server using below link

```
sudo -i

https://pwittrock.github.io/docs/tasks/tools/install-kubectl/

```

```
root@ip-172-31-93-232:~# kubectl version --client
Client Version: v1.28.2
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
```

- allow 6443 port for jenkins server to commuicate with k8s master server
  
CustomTCP	   TCP	   6443	      172.31.0.0/16

also add below address details in /etc/hosts file of jenkins, because jenkins server dont know hostname of k8s master, incase you have added below addresses hosts file

sudo vi /etc/hosts
```
172.31.92.81    k8smaster.example.net  k8smaster
172.31.91.41    k8sworker1.example.net k8sworker1
172.31.95.218   k8sworker2.example.net k8sworker2
```

```
sudo su - jenkins
```

Create config file and copy config file content from Kubernetes Cluster master machine to jenkins user home directory
and save the content.

vi .kube/config

```
jenkins@ip-172-31-93-232:~$ kubectl get nodes
NAME                     STATUS   ROLES           AGE   VERSION
k8smaster.example.net    Ready    control-plane   13d   v1.28.2
k8sworker1.example.net   Ready    <none>          13d   v1.28.2
k8sworker2.example.net   Ready    <none>          13d   v1.28.2
```
- the above command is executed from jenkins server

We can use kubectl commands directly in pipeline script , kubectl commands will get executed in Kubernetes cluster directly.

```
stage("Deploy To Kuberates Cluster"){
  sh "kubectl apply -f springBootMongo.yml"
}
```

- iam going with approach 2 that is installing kubectl in jenkins server and executing kubectl commands in pipeline



JENKINS JOB SETP:
-----------------

![image](https://github.com/vijay2181/jenkins-kubernetes-integration/assets/66196388/8f4b49e9-b788-4ba0-8451-28edc3e7cbe7)


- SETUP MAVEN FROM GLOBAL TOOL CONFIGURATION
  
- Dashboard > Manage Jenkins > Tools

![image](https://github.com/vijay2181/jenkins-kubernetes-integration/assets/66196388/e4390dd2-0936-4d14-b493-5679c3c4b348)

- copy the pipeline script from JenkinsfileKubernetes and paste in jenkins groovy sandbox
  
- app is deploying in default namespace of kubernetes

- build the pipeline


```
jenkins@ip-172-31-93-232:~/springboot-mongo-docker$ kubectl get pods
NAME                                   READY   STATUS    RESTARTS   AGE
mongodbrs-mv8wc                        1/1     Running   0          41s
springappdeployment-6c76d99959-8k7sx   1/1     Running   0          41s
springappdeployment-6c76d99959-8xtpp   1/1     Running   0          41s
```

```
jenkins@ip-172-31-93-232:~/springboot-mongo-docker$ kubectl get svc
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP        13d
mongo        ClusterIP   10.107.26.13    <none>        27017/TCP      69s
springapp    NodePort    10.107.17.200   <none>        80:30754/TCP   69s
```

- take any worker node publicip and access it

![image](https://github.com/vijay2181/jenkins-kubernetes-integration/assets/66196388/f75c383a-34a4-4db4-952b-3c0bf77c53a5)

![image](https://github.com/vijay2181/jenkins-kubernetes-integration/assets/66196388/7fd8ea61-d6e7-4570-94ae-163ce07284e6)






























