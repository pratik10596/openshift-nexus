# Setup Nexus Repo

Nexus Repository is the central source of control to efficiently manage all binaries and build artifacts across your DevOps pipeline. 

## Prerequisites

1) Install Nexus Operator in namespace

2) Create separate Service Account for Nexus

    User Management -> Service Accounts -> Create ServiceAccount

3) Now attach custom scc

```bash
oc apply -f nexus-custom-scc.yaml

oc adm policy add-scc-to-user nexus-custom-scc -z <nexus-service-account>
```

## Create Nexus Repo Application

1) Switch to Administrator Section

2) Click on Operators & select Nexus Repository Operator

3) Click on NexusRepo tab -> Create NexusRepo

4) Select yaml view -> change app name 

5) Change NEXUS_SECURITY_RANDOMPASSWORD to true

6) Click on Create Button

## Expose Route for Nexus Repo

1) Switch to Administrator Section

2) Click on Networking -> Routes -> Create Route -> Enter route name -> From service list select nexus repo

3) Enable security route -> Termination Type=Edge -> Insecure Traffic=Redirect 

4) Click on Create button 

## Create Nexus Repository 

1) Now open nexus console & follow the on screen instructions to setup repo

2) Click repositories -> Create Repository -> Select Raw Hosted ->  Enter Name=artisan & click on Create Repository button

## Change Service Account

By default Nexus Repo will get create under "default service account". We need to change it to the service account that we have created with custom scc

1) Click on app -> Click on Edit Deployment -> Go to yaml file

2) Under spec section add below code

```bash
serviceAccount: <nexus-service-account>
```

3) Nexus application deployment will get start within few seconds