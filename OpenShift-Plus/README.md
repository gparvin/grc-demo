# ACS ACM Dev Conference

The "IBM Digital Developer Conference (DDC): Cloud Security" is an online conference by developers for developers to learn and build secure enterprise workloads in the open hybrid cloud. Top experts in cloud security, innovating client partners, and experienced peers in cybersecurity share their insights and learnings. 

## Overview

What are some of the challenges of security, governance and compliance in the cloud:
- Enterprise clients need to meet internal enterprise security standards as well as external regulatory compliance requirements
- Enterprise clients need to go through periodic audits by external auditors of their IT infrastructure
- Securing clouds typically requires new security tools in addition to existing security tools used for traditional non-cloud IT infrastructures
- Cloud environments are dynamic 
- Enterprises typically use more than one cloud provider
- Enterprises require interaction with existing systems of record and data
- Customers want ‘open’ not proprietary solutions 

Governance using OpenShift Plus takes advantage of the strengths found in several of Red Hat’s products.  

Red Hat Advanced Cluster Management for Kubernetes helps with cluster management, application management and also contains a policy framework that helps with configuration and governance.

---
<div style="align:center"><img alt="Click the link for the central route" src="images/acm-policy-arch.png" width="60%"></div>
---

OpenShift’s Compliance Operator helps you apply compliance rules to your clusters.  And using the Compliance Operator with ACM and ACS simplifies the process when handling this for many clusters.

---
<div style="align:center"><img alt="Click the link for the central route" src="images/ocp-compliance-arch.png" width="60%"></div>
---

Advanced Cluster Security provides not only deep insights into the security of your clusters, it helps you visualize and prioritize your security concerns.

---
<div style="align:center"><img alt="Click the link for the central route" src="images/acs-arch.png" width="60%"></div>
---

Using these three products your security posture across many clusters can be well understood and managed.


## Prerequisites

ACM 2.4 is already installed on an OpenShift 4.9 cluster.  All of the resources used in this demonstration are located in this [demo repository](https://github.com/gparvin/grc-demo) in the `OpenShift-Plus` subdirectory.

- All commands that are executed must in in the context of the ACM hub cluster.
- An ACM Credential was pre-created to allow the new managed cluster to be created without having to create a new credential.
- Login to the OpenShift Container Platform web console for the ACM hub cluster

## Procedure

### Deploy Operators for Policy Enforcement

Deploy the Policy Enforcement Points discussed in the architecture.  This includes many operators including the Compliance Operator and the Advanced Cluster Security Operator.

```
oc create ns operators
cd deploy
./deploy.sh -u https://github.com/gparvin/grc-demo -b main -p OpenShift-Plus/operators -a plus -s medium -n operators
```

Login to the Advanced Cluster Security Central Server to create an API token and use the token to deploy the init bundle secrets.

1. Open the Web Console for OpenShift
2. Navigate to `Networking` then `Routes` and select the link for the `Route` named `central` in the `stackrox` namespace.
3. In the Advanced Cluster Security web console, specify the username `admin`
4. Switch back to the OpenShift web console to find the password.
5. Select `Workloads` then `Secrets` then find and select the secret named `central-htpasswd` in the `stackrox` namespace.
6. Select the `Copy to clipboard` icon next to the `password` Data item.
7. Switch back to the ACS web console and paste the password into the login prompt
8. Select `Platform Configuration` then `Integrations`, find the `API Token` integration and select it
9. Select `Generate Token` and specify a Token name and Role, then select `Generate`
10. Copy the generated token by selecting the copy icon.
11. In your command shell create an environment variable with the token by running: `export ROX_API_TOKEN=<paste token value here>`

Next you must run the script to create and deploy the ACS init bundles.  In the same command window, run the command below:

```
cd ../acs-bundle
./deploy-bundle.sh -i bundle.yaml | oc apply -f -
```

### Deploy policies to determine compliance

Install the Policies for compliance.  Included Policies are:
- FedRAMP Moderate policy
- Advanced Cluster Security SecuredClusters policy
- Etcd encryption

```
cd ../deploy
oc create ns compliance
./deploy.sh -u https://github.com/gparvin/grc-demo -b main -p OpenShift-Plus/compliance -a compliance -s medium -n compliance
```

Wait for the policies to become compliant. Run a Compliance scan in the ACS Central Server

### Create the Pacman application

The Pacman application is in the OpenShift-Plus/pacman subfolder.  Use the following steps to create this Application in ACM.

1. In the ACM web console, select 

Open the Compliance scan results for the Moderate OpenShift 
View Compliance scan results in ACS and note the compliance percentages
Show Compliance issues and identify a “resource” update that can resolve the issue
Deploy the remediation policies



### Network Policies

Use ACS to create a Network Policy for the Application
Copy the NetworkPolicy to the application repository
Update the application

### Deploy a new cluster

Run the Compliance Operator scan and the ACS Compliance scans again
Show the updated compliance percentages


### Runtime Detections

Stop pods from installing new packages
Apply the right policy in ACS
THe pacman pod is running Debian
Exec into the pacman pod and try running apt
Time permitting: Handling pod intrusions.  Prevent usage of package tooling.
oc get event | grep StackRox
LAST SEEN   TYPE      REASON                    OBJECT        MESSAGE
37s          Warning StackRox enforcement   job/sec-eng   A pod (sec-eng-7bj55) violated StackRox policy "Red Hat Package Manager Execution" and was killed

View the Compliance of the new Cluster

## Conclusions

Using ACM, ACS and the Compliance Operator together you can not only dig deep into security and compliance issues on your cluster, but you can make sure new clusters you provision are automatically protected too.  Using ACS, ACM and the Compliance Operator together helps ease the burden of governance and get you into a best practice of continuous security readiness.

## Useful Resources

[Installing Advanced Cluster Security using Advanced Cluster Management](https://github.com/open-cluster-management/advanced-cluster-security)

[Community policies for Advanced Cluster Management](https://github.com/open-cluster-management/policy-collection)

[Using the Compliance Operator](https://github.com/openshift/compliance-operator)


