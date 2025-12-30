---
title: "Kubernetes Basic Commands: Pods, Labels, and Real‑Time Monitoring"
date: 2025-12-30T14:14:38+05:30
draft: false
---
Introduction
When working with Kubernetes (or OpenShift), understanding how to watch pods, apply label filters, and perform bulk operations is essential.
This guide walks through practical commands you can use every day to check pod status, filter resources using labels, and get more visibility into your cluster.
Monitoring Pods in Real Time
Using the -w (watch) option keeps the command open and updates automatically when pod states change.
Example:
oc get pods -w
If a pod changes from Pending to Running, it appears immediately as a new line.
Bulk Operations Using Labels
Labels make it easy to group and manage Kubernetes resources.
Delete all pods labeled env=test:
oc delete pods -l env=test
Describe pods belonging to app=web:
oc describe pods -l app=web
Viewing Labels on Pods
Show all labels:
kubectl get pods --show-labels
Filter pods with -l:
kubectl get pods -l app=web
Show label columns with -L:
kubectl get pods -L environment,app,tier
Combine -l and -L:
oc get pods -l app=web -L environment
Visual Diagram: How Labels and Selectors Work
Below is a simple diagram to help visualize how Kubernetes matches labels with selectors.
Pods with labels:
+-------------------------------+
| Pod: my-nginx-pod | | Labels: | | app=web | | environment=dev |

+-------------------------------+
+-------------------------------+
| Pod: backend-api | | Labels: | | app=api | | environment=prod |

+-------------------------------+
+-------------------------------+
| Pod: nginx-green | | Labels: | | app=web | | environment=prod |

+-------------------------------+
Label selector example:
Selector: app=web
The selector matches only the pods that contain label app=web.
Diagram of filtering:
Selector (app=web)
|
+---------------------------+
| |

Matches: my-nginx-pod Matches: nginx-green
Does not match: backend-api
(because app=api)
What this means:
• Kubernetes does not care about names.
• It matches pods based on labels only.
• If a pod has the label requested by the selector, it is included.
• If not, it is ignored.
This logic works the same for:
• Deployments
• ReplicaSets
• Services
• Jobs
• Network policies
Conclusion
These commands and concepts help you manage pods more effectively in Kubernetes or OpenShift.
Real‑time monitoring, label filtering, and improved visibility make troubleshooting easier and faster.