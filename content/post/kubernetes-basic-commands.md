---
title: "Kubernetes Basic Commands"
date: 2025-12-30T14:14:38+05:30
draft: false

Monitor your pods in real-time
When you add -w(watch), the command stays open and waits for changes.
Continuous Updates: If a pod’s status changes (e.g., from Pending to Running), a new line will automatically appear in your terminal.

ved@master:~$ oc get pods -w
NAME READY STATUS RESTARTS AGE
httpd-pod 1/1 Running 0 11m
my-nginx-pod 1/1 Running 1 (4h13m ago) 16h
my-nginx-web 1/1 Running 1 (4h13m ago) 15h
nginx-pod 1/1 Running 1 (4h13m ago) 16h
nginx-pod-new 0/1 ContainerCreating 0 12s
nginx-pod-new 1/1 Running 0 16s

ved@master:~/k8-yaml$ oc get pods -w -o wide
NAME READY STATUS RESTARTS AGE IP NODE NOMINATED NODE READINESS GATES
httpd-pod 1/1 Running 0 24m 10.244.2.7 w2.mylab.local
my-nginx-pod 1/1 Running 1 (4h26m ago) 16h 10.244.1.12 w1.mylab.local

Bulk Operations with Labels:

Delete all “test” pods:


ved@master:~$ oc delete pods -l env=test

Describe all pods for a specific app:
ved@master:~$ oc describe pods -l app=web

1. Show labels for all the pods

ved@master:~$ kubectl get pods --show-labels
NAME READY STATUS RESTARTS AGE LABELS
httpd-test-pod 1/1 Running 0 162m app=test-web
my-nginx-pod 1/1 Running 1 (174m ago) 15h app=web
my-nginx-web 1/1 Running 1 (174m ago) 14h app=green
nginx-pod 1/1 Running 1 (174m ago) 15h app=web

2. The lowercase -l stands for selector (label selector). It acts as a filter.

Analogy: It’s like searching for “All employees who work in the Sales department.”


ved@master:~$ kubectl get pods -l app=web
NAME READY STATUS RESTARTS AGE
my-nginx-pod 1/1 Running 1 (175m ago) 15h
nginx-pod 1/1 Running 1 (175m ago) 15h

3.The uppercase -L stands for label columns. It adds new columns to your output. Used For Visibility (Show more data)

Action: It tells Kubernetes to take the values of the labels environment, app, and tier and display them as separate columns in the terminal.


ved@master:~$ kubectl get pods -L environment,app,tier
NAME READY STATUS RESTARTS AGE ENVIRONMENT APP TIER
httpd-test-pod 1/1 Running 0 3h30m test-web
my-nginx-pod 1/1 Running 1 (3h42m ago) 15h web non-prod
my-nginx-web 1/1 Running 1 (3h42m ago) 15h prod green
nginx-pod 1/1 Running 1 (3h42m ago) 15h web

4.If you want to see only your web pods and also want to know which environment they belong to at a glance, you can combine them( -l and -L)

ved@master:~$ oc get pods -l app=web -L environment
NAME READY STATUS RESTARTS AGE ENVIRONMENT
httpd-pod 1/1 Running 0 5m40s dev
my-nginx-pod 1/1 Running 1 (4h8m ago) 16h
my-nginx-web 1/1 Running 1 (4h8m ago) 15h prod
nginx-pod 1/1 Running 1 (4h8m ago) 16h non-prod


---

