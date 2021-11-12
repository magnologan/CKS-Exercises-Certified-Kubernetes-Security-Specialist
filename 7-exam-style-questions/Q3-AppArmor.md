### Question - AppArmor

A pod has been created in the spectacle namespace. However, there are a couple of issues with it:
- The pods has been created with privileged permissions
- it allows read access 

- Use the AppArmor profile created at /etc/apparmor.d/database.
- create a service account in the spectacle namespace called "test-sa" and use this service account on the pod

#### 1 - Load the AppArmor profile

```sh

apparmor_parser -q /etc/apparmor.d/database

```

#### 2 - Create a pod using the AppArmor profile backend

```sh

apiVersion: v1
kind: Pod
metadata:
  annotations:
    container.apparmor.security.beta.kubernetes.io/nginx: localhost/database #Apply profile 'restricted-fronend' on 'nginx' container
  labels:
    run: nginx
  name: apparmor-pod
  namespace: spectacle
spec:
  serviceAccount: test-sa 
  containers:
  - image: nginx:alpine
    name: nginx
    volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: test-volume
  volumes:
  - name: test-volume
    hostPath:
      path: /data/pages
      type: Directory

```
