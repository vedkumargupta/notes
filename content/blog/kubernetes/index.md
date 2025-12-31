---
title: "Kubernetes Basic commands"
date: 2025-12-30T14:14:38+05:30
draft: false
---

---
Monitor your pods in real-time
When you add -w (watch), the command stays open and waits for changes. Continuous Updates: If a pod’s status changes (e.g., from Pending → Running), a new line automatically appears in your terminal.

---
Watch pods
bash -8 lines

ved@master:~$ oc get pods -w
NAME            READY   STATUS              RESTARTS        AGE
httpd-pod       1/1     Running             0               11m
my-nginx-pod    1/1     Running             1 (4h13m ago)   16h
my-nginx-web    1/1     Running             1 (4h13m ago)   15h
nginx-pod       1/1     Running             1 (4h13m ago)   16h
nginx-pod-new   0/1     ContainerCreating   0               12s
nginx-pod-new   1/1     Running             0               16s


Watch pods with wide output
bash -4 lines

ved@master:~/k8-yaml$ oc get pods -w -o wide
NAME            READY   STATUS              RESTARTS        AGE   IP            NODE             NOMINATED NODE   READINESS GATES
httpd-pod       1/1     Running             0               24m   10.244.2.7    w2.mylab.local   <none>           <none>
my-nginx-pod    1/1     Running             1 (4h26m ago)   16h   10.244.1.12   w1.mylab.local   <none>           <none>


---
Bulk Operations with Labels
Delete all "test" pods
bash -1 line

ved@master:~$ oc delete pods -l env=test


Describe all pods for a specific app
bash -1 line

ved@master:~$ oc describe pods -l app=web


---
1. Show labels for all pods
bash -6 lines

ved@master:~$ kubectl get pods --show-labels
NAME             READY   STATUS    RESTARTS       AGE    LABELS
httpd-test-pod   1/1     Running   0              162m   app=test-web
my-nginx-pod     1/1     Running   1 (174m ago)   15h    app=web
my-nginx-web     1/1     Running   1 (174m ago)   14h    app=green
nginx-pod        1/1     Running   1 (174m ago)   15h    app=web


---
2. Using -l (label selector)
The lowercase -l stands for selector (label selector). It acts as a filter. Analogy: Like searching for “all employees who work in the Sales department.”

bash -4 lines

ved@master:~$ kubectl get pods -l app=web
NAME           READY   STATUS    RESTARTS       AGE
my-nginx-pod   1/1     Running   1 (175m ago)   15h
nginx-pod      1/1     Running   1 (175m ago)   15h


---
3. Using -L (show label columns)
The uppercase -L adds label values as new columns in your output. It tells Kubernetes to take the label values (environment, app, tier) and display them as separate columns.

bash -6 lines

ved@master:~$ kubectl get pods -L environment,app,tier
NAME             READY   STATUS    RESTARTS        AGE     ENVIRONMENT   APP        TIER
httpd-test-pod   1/1     Running   0               3h30m                 test-web
my-nginx-pod     1/1     Running   1 (3h42m ago)   15h                   web        non-prod
my-nginx-web     1/1     Running   1 (3h42m ago)   15h     prod          green
nginx-pod        1/1     Running   1 (3h42m ago)   15h                   web


---
4. Combine -l and -L
Filter only web pods and show environment labels.

bash -6 lines

ved@master:~$ oc get pods -l app=web -L environment
NAME           READY   STATUS    RESTARTS       AGE     ENVIRONMENT
httpd-pod      1/1     Running   0              5m40s   dev
my-nginx-pod   1/1     Running   1 (4h8m ago)   16h
my-nginx-web   1/1     Running   1 (4h8m ago)   15h     prod
nginx-pod      1/1     Running   1 (4h8m ago)   16h     non-prod


---