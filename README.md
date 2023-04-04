# keycloak

## Prerequisites
* Docker installed
* Minikube installed

## Run Kubernetes
\# Start local Kubernetes cluster <br />
`minikube start`<br />
`minikube addons enable ingress` <br />

\# Add helm repo bitnami and ingress <br />
`helm repo add bitnami https://charts.bitnami.com/bitnami`<br />
`helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx`<br />

\# Install ingress-nginx on Kubernetes cluster <br />
`helm install ingress-nginx -n ingress-nginx --create-namespace ingress-nginx/ingress-nginx`<br />

\# Create separate namespace for keycloak <br />
`kubectl create ns keycloak`<br />

\# Apply config map <br />
`kubectl apply -f cm.yml`<br />

## Install Infinispan Cluster
\# Make docker build <br />
`docker build -t my_repo/ispn:version .`<br />
`docker push my_repo/ispn:version`

\# Use created and pushed image in deployment <br />
![image](ispn_image.png)

\# Install postgres database on Kubernetes cluster for infinispan <br />
`helm install -n keycloak ispn-db bitnami/postgresql-ha --set postgresql.replicaCount=1`
\# To export db password <br />
`export POSTGRES_PASSWORD=$(kubectl get secret --namespace keycloak ispn-db-postgresql-ha-postgresql -o jsonpath="{.data.password}" | base64 -d)` <br />
`echo $POSTGRES_PASSWORD` <br />
then put dabase password like shown on below picture
![image](ispn_db.png)

\# Apply deployment and service of infinispan (navigate to ispn catalog) <br />
`kubectl apply -f infinispan-deployment.yaml`<br />
`kubectl apply -f infinispan-service.yaml`<br />

## Install Keycloak Cluster
\# Make docker build <br />
`docker build -t my_repo/keycloak:version .`<br />
`docker push my_repo/keycloak:version`

\# Use created and pushed image in deployment <br />
![image](kc_image.png)

\# Generate key and cert for ssl communication with keycloak <br />
`openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout auth-tls.key -out auth-tls.crt -subj "/CN=auth.localtest.me/O=keycloak"`<br />

\# Put key and cert on secret space on Kubernetes <br />
`kubectl create secret -n keycloak tls auth-tls-secret --key auth-tls.key --cert auth-tls.crt`<br />

\# Install postgres database on Kubernetes cluster for keycloak<br />
`helm install -n keycloak keycloak-db bitnami/postgresql-ha --set postgresql.replicaCount=1`

\# To export db password <br />
`export POSTGRES_PASSWORD=$(kubectl get secret --namespace keycloak keycloak-db-postgresql-ha-postgresql -o jsonpath="{.data.password}" | base64 -d)` <br />
`echo $POSTGRES_PASSWORD` <br />
then put dabase password like shown on below picture
![image](kc_db.png)

\# Apply deployment and service of infinispan (navigate to ispn catalog) <br />
`kubectl apply -f keycloak-deployment.yaml`<br />
`kubectl apply -f keycloak-ingress.yaml -n keycloak`<br />

