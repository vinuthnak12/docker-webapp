Types of volumes.
EmptyDir

Deployment.yaml 

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flm
spec:
  replicas: 2
  selector:
    matchLabels:
      app: swiggy
  template:
    metadata:
      labels:
        app: swiggy
    spec:
      containers:
        - name: cont-1
          image: ubuntu
          command: ["/bin/bash", "-c", "while true; do echo welcome to devops; sleep 5; done"]
          volumeMounts:
            - name: devops
              mountPath: "/tmp/jenkins"

        - name: cont-2
          image: ubuntu
          command: ["/bin/bash", "-c", "while true; do echo welcome to devops; sleep 5; done"]
          volumeMounts:
            - name: devops
              mountPath: "/tmp/docker"

      volumes:  # ✅ Now correctly aligned
        - name: devops
          emptyDir: {}












nano Deployment.yaml
kubectl create -f Deployment.yaml
kubectl get po
kubectl exec -it flm-6676b4487-fdlws -- bash
kubectl exec -it flm-6676b4487-fdlws -c cont-1 -- bash
kubectl exec -it flm-6676b4487-h97r8 -c cont-2 -- bash
kubectl exec -it flm-6676b4487-h97r8 -c cont-1 -- bash
kubectl delete pod flm-6676b4487-fdlws

Step1 : create the deployment.yaml file
Step2 : Enter into the containers and check path is mounted in the both the containers.
Step3 : Enter into the conatiner 1 and create file and check in the conatiner same vice versa in the both.
Step4 : Delete the pod and check once able to find the previous files in this pod. Files will not be reflected.
Step5 : In Empty dir once pod is deleted volume data is not reflected in new pod.

Volume is attached to pod in Empty dir.




HOSTPATH :

This Volume type is the advanced version of the previous volume type
Volume is attached to host.
Here volume is not attached to pod.
Here volume is attached to serever.
Volume is attached to host.
Even pod is deleted we dont loose any data.
Volume is attached to server.
Data will come to new pod.
Volume is attached to host.

volumes:  # ✅ Now correctly aligned
        - name: devops
          hostPath:
            path: "/opt/myvolume"


Deployment.yaml


---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flm
spec:
  replicas: 2
  selector:
    matchLabels:
      app: swiggy
  template:
    metadata:
      labels:
        app: swiggy
    spec:
      containers:
        - name: cont-1
          image: ubuntu
          command: ["/bin/bash", "-c", "while true; do echo welcome to devops; sleep 5; done"]
          volumeMounts:
            - name: devops
              mountPath: "/tmp/jenkins"

        - name: cont-2
          image: ubuntu
          command: ["/bin/bash", "-c", "while true; do echo welcome to devops; sleep 5; done"]
          volumeMounts:
            - name: devops
              mountPath: "/tmp/docker"

      volumes:  # ✅ Now correctly aligned
        - name: devops
          hostPath:
            path: "/opt/myvolume"



Commands :  check all the senarios delete the pod and check the files. 

kubectl get deploy
kubectl get po
kubectl get rs
kubectl create -f Deployment.yaml
kubectl exec -it flm-967bffbdb-6b75b -c cont-1 -- bash
kubectl exec -it flm-967bffbdb-6b75b -c cont-2 -- bash
kubectl delete pod flm-967bffbdb-6b75b
kubectl exec -it flm-967bffbdb-82g76 -c cont-2 -- bash
nano Deployment.yaml
ll
cd opt
cat Deployment.yaml


PERSISTENT VOLUME :

persistent meanse always avaialble.
Persistent volume is an adavanced version of EmptyDir and hostPath volume
types.
Persistent  volume does not store the data over local server.

