Deployment Strategy: Recreate
---
This repository demonstrates a Recreate deployment strategy using Kubernetes. The application is deployed in a namespace called team4 and showcases a simple HTTP echo server that outputs different feature availability based on updates.

---
Repository Structure
---

Namespace Configuration:
---
Defines a team4 namespace to isolate the deployment.

Deployment Configurations:
---
Three separate deployment configurations for team4-app, each illustrating an update to the application's features:
1. Features available: USD, SGD
2. Features available: USD, SGD, JPY
3. Features available: USD, SGD, JPY, EUR
Uses the Recreate strategy to demonstrate how the old pods are terminated before new pods are created.

Service Configuration:
---
Exposes the team4-app deployment as a service (team4-app-svc) on port 80.

Utility Pod:
---
Includes a dnstools pod for testing DNS resolution within the namespace.

Deployment Steps
---
1. Clone the Repository:
</BR>git clone https://github.com/HninYuKhaing/DeploymentStrategyRecreate.git
</BR>cd DeploymentStrategyRecreate
2. Deploy the Application:
</BR>kubectl apply -f deployment1.yaml 
3. Create the Service:
</BR>kubectl apply -f service.yaml 
4. Launch Utility Pod:
</BR>kubectl apply -f dnstools.yaml

Access the Application
---
Get the Deployment, Service details:
</BR>Open new terminal
</BR>watch kubectl get all -n team4 -o wide

Testing DNS with dnstools
---
</BR><B>Open new terminal</B>
</BR>kubectl exec -it pod/dnstools -n team4 -- sh
</BR>while true; do curl http://team4-app-svc; sleep 0.5; done

</BR><B>Open new terminal</B>
</BR>watch kubectl rollout history  deployment/team4-app -n team4

</BR><B>Open new terminal</B>
</BR>watch kubectl rollout status  deployment/team4-app -n team4

</BR><B>Open new terminal</B>
</BR>Deploy the New Application:
</BR>kubectl apply -f deployment2.yaml
</BR>Look for the changes in other terminals

</BR><B>Open new terminal</B>
</BR>Deploy the New Application:
</BR>kubectl apply -f deployment3.yaml
</BR>Look for the changes in other terminals

</BR><B>Undo changes to previous</B>
</BR>kubectl rollout undo deployment/team4-app -n team4

</BR><B>Undo changes to revision-1</B>
</BR>kubectl rollout undo deployment/team4-app -n team4 --to-revision=1

Cleanup
---
kubectl delete namespace team4

Key Features of Recreate Strategy
---
- Ensures zero overlap between old and new pod versions.
- Suitable for scenarios where only one version of the application should run at a time.
- Downtime during the deployment.

YouTube Demo:
---
https://youtu.be/Wl26QaOnMh8




