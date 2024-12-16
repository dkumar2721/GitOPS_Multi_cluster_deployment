# GitOPS_Multi_cluster_deployment
my projects_5. 

In this project, I implemented the deployment of multi-cluster Infracture using GitOps principles.

I created 3 clusters. Then, I used ARGO-CD tools with hub-spoke argo-cd deployment strategies to deploy my application in the spoke cluster using a centralized cluster.

#stages involved in this project:

   ##cluster creation
   ##Instal  Argo-CD
   ##verify login
   ##ADD cluster
   ##Deploying the application in the cluster 
#pre-request:
  1)aws-cli installed and configure
  2)eksctl-cli installed
  3)kubectl -installed

EKS Clusters Creation
eksctl create cluster --name hub-cluster --region us-west-1

eksctl create cluster --name spoke-cluster-1 --region us-west-1

eksctl create cluster --name spoke-cluster-2 --region us-west-

![Screenshot 2024-12-15 180214](https://github.com/user-attachments/assets/b308a306-a1a7-48d7-b70a-faaa3db6917a)

clusters are created.

list the context 
  ...
    kebectl config get-contexts
  ...
use the context
...
kubectl config use-context cluster-context name
...

To check the current context
...
  kubectl config current-context

...


Now we got hub or centralized server .



#INSTALL ARGOCD

...

kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
...

need to change cluster-ip mode to NodePort
...
kubectl get svc -n argocd
...
then select the select and copy the argocd-server

then,
...
kubectl edit svc argocd-server -n argocd 
...

change cluster ip to  Nodeport and save.


we need to login at argocd in insecure mode .

...
check with offical documentation
...

next we need to login in argocd UI

..

kubectl get pod -n argocd 
kubectl get svc-n argocd 
...
tke the  argocd-server ip+port .

put that in a browser. You will get a UI page and end with login page of argo cd 

username : admin

password : we need to get .

...

kubectl get secrets -n argocd 

kubectl  edit secret argocd-intial-admin-secret -n argocd
...

copy the password  and 

...
echo copied-password | base64 --decode
...
then u will get a new password. with that password log in to Argo-cd.


#download the argocd-cli from official documentation.

login to argocd using terminal :
...
argocd login ip-address+nodeport
...

enter the user details and get it 

##ADD the cluster and configure to hub cluster 

....
argo cluster add context of cluster-1  --server ip-address+ port
argo cluster add context of cluster-2  __server ip-address +port
...


![Screenshot 2024-12-15 235111](https://github.com/user-attachments/assets/bc91ecfd-fc9d-483c-a7c9-8afe03f0c88e)


#Now the cluster are ready to deployment.

using the ui deploy the application from the git-repo 

![Screenshot 2024-12-16 004150](https://github.com/user-attachments/assets/ee25fe70-5eaf-4438-ad6d-a399e4ac003e)

Finally any change to a given repo the argocd will automatically create an infrastructure and auto-heal. even the developer makes any changes in the cluster also its gets the notified.




















    



