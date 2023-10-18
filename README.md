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

