# MongoDB StatefulSet on Kubernetes with Flask CRUD Application

This guide walks you through deploying a MongoDB StatefulSet on Kubernetes and a Flask application to interact with it. We'll perform CRUD operations on MongoDB using the Flask app.

## Prerequisites
Ensure you have the following installed:
- Kubernetes (with kubectl configured)
- A compatible storage class for MongoDB persistence

## Steps to Deploy MongoDB and Flask App

### 1. Apply Kubernetes Configurations
Run the following commands to set up the namespace, service account, secrets, MongoDB StatefulSet, and Flask app:

```bash
kubectl apply -f namespace.yaml
kubectl apply -f service-account.yaml
kubectl apply -f secret.yaml
kubectl apply -f mongo-svc.yaml
kubectl apply -f mongo-db-stateful-1.yaml
kubectl apply -f app-service.yaml
kubectl apply -f app.yaml
2. Port-Forward Flask App
Once the Flask app is deployed, port-forward it to make it accessible on your local machine:

bash
Copy code
kubectl port-forward service/flaskapp 4040:5000
You can now access the Flask app at http://localhost:4040.

3. Switch to the Database Namespace
Switch to the database namespace to view MongoDB and Flask application resources:

bash
Copy code
kubens database
4. Verify the Deployment
To check the status of your MongoDB and Flask application, run:

bash
Copy code
kubectl get all
Example output:

sql
Copy code
NAME                         READY   STATUS    RESTARTS   AGE
pod/flaskapp-7bb55db7b5-zmw5l   1/1     Running   0          41s
pod/mongodbflask-0              1/1     Running   0          12m

NAME                  TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)         AGE
service/flaskapp      ClusterIP   172.20.153.153   <none>        5000/TCP        62s
service/mongodb-headless ClusterIP None             <none>        27017/TCP       3h22m

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/flaskapp  1/1     1            1           43s

NAME                                DESIRED   CURRENT   READY   AGE
replicaset.apps/flaskapp-7bb55db7b5 1         1         1       43s

NAME                   READY   AGE
statefulset.apps/mongodbflask  1/1     12m
5. Connect to MongoDB and Perform CRUD Operations
To connect to MongoDB using a temporary pod, use the following command:

bash
Copy code
kubectl run -it mongo-shell --image=mongo:4.0.17 --rm -- /bin/bash
Once inside the MongoDB shell, connect to the MongoDB instance:

bash
Copy code
mongo --host mongodbflask-0.mongodb-headless.database.svc.cluster.local -u mongodbuser -p password123 --authenticationDatabase admin
Create Database and Collection
Switch to the philips database and view the collections:

javascript
Copy code
use philips;
show collections;
Sample CRUD Operation - Read Data
Retrieve records from the people collection:

javascript
Copy code
db.people.find();
Example output:

json
Copy code
{
  "_id" : ObjectId("652e9f3a9954ce67534edbaa"),
  "username" : "javvaji hari",
  "firstname" : "Kishore",
  "surname" : "Javvaji",
  "email" : "hkishore42@example.com",
  "score" : 0
}
Exit the MongoDB shell:

bash
Copy code
exit
Cleanup
To delete all resources, run:

bash
Copy code
kubectl delete -f namespace.yaml -f service-account.yaml -f secret.yaml -f mongo-svc.yaml -f mongo-db-stateful-1.yaml -f app-service.yaml -f app.yaml
Conclusion
You’ve successfully deployed a MongoDB StatefulSet on Kubernetes and interacted with it using a Flask app to perform CRUD operations.




















# eks-mongo-crud-flask
Perform crud operations on mongodb

kubectl apply -f namespace.yaml
kubectl apply -f service-account.yaml
kubectl apply -f secret.yaml
kubectl apply -f mongo-svc.yaml
kubectl apply -f mongo-db-stateful-1.yaml
kubectl apply -f app-service.yaml
kubectl apply -f app.yaml
kubectl port-forward service/flaskapp 4040:5000

kubens database

310229972@YY525386 MINGW64 ~/.aws/mongo-k8
$ kubectl get all
NAME                            READY   STATUS    RESTARTS   AGE
pod/flaskapp-7bb55db7b5-zmw5l   1/1     Running   0          41s
pod/mongodbflask-0              1/1     Running   0          12m

NAME                       TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)     AGE
service/flaskapp           ClusterIP   172.20.153.153   <none>        5000/TCP    62s
service/mongodb-headless   ClusterIP   None             <none>        27017/TCP   3h22m

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/flaskapp   1/1     1            1           43s

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/flaskapp-7bb55db7b5   1         1         1       43s

NAME                            READY   AGE
statefulset.apps/mongodbflask   1/1     12m


kubectl run -it mongo-shell --image=mongo:4.0.17 --rm -- /bin/bash

root@mongo-shell:/# mongo --host  mongodbflask-0.mongodb-headless.database.svc.cluster.local -u mongodbuser -p password123 --authenticationDatabase admin

> use philips;
switched to db philips
> show collections;
people

> db.people.find()
{ "_id" : ObjectId("652e9f3a9954ce67534edbaa"), "username" : "javvaji hari", "firstname" : "Kishore", "surname" : "Javvaji", "email" : "hkishore42@example.com", "score" : 0 }
> exit;

