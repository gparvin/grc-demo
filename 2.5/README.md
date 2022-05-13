# ACM 2.5 Demo Setup

Here's a quick procedure to get setup with our ACM 2.5 features.

## Setup Subscription Administrator

Make sure you are a suscription-admin.  If you're using `kubeadmin`, just apply this configuration:

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: open-cluster-management:subscription-admin
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: open-cluster-management:subscription-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: system:admin
```

## Setup Cluster Namespace Bindings

The `policies` namespace needs to be associated with the `default` managed cluster set.  To do that, create
the policies namespace `oc create ns policies` and then create this resource:

```
apiVersion: cluster.open-cluster-management.io/v1beta1
kind: ManagedClusterSetBinding
metadata:
  name: default
  namespace: policies
spec:
  clusterSet: default
```

## Deploy Policies

Use the deploy script in the `deploy` subdirectory to deploy policies and policy sets.  The commands below
install our sample policy sets, our sample generator created policies and a few extra policies that all
display different features in GRC.

```
cd deploy
echo y | ./deploy.sh -u https://github.com/gparvin/grc-demo.git -p 2.5/policy-sets/acm-hardening -n policies -a acm
echo y | ./deploy.sh -u https://github.com/gparvin/grc-demo.git -p 2.5/policy-sets/openshift-hardening -n policies -a ocp
echo y | ./deploy.sh -u https://github.com/gparvin/grc-demo.git -p 2.5/policy-sets/openshift-plus -n policies -a acs
echo y | ./deploy.sh -u https://github.com/gparvin/grc-demo.git -p 2.5/policy-sets/kustomize -n policies -a sample
echo y | ./deploy.sh -u https://github.com/gparvin/grc-demo.git -p 2.5/policies -n policies -a policies
```

## Add a managed cluster

To fully show some functionality we need to create a managed cluster.  It's quick and easy to create an AWS credential
and then a new cluster.  The OpenShift Plus PolicySet that install ACS uses the secure hub templates only for 
non-hub managed clusters.

## Links to features

Which policies detail which features in ACM?  Here's a list of a feature and you can navigate to that feature with 
these hints:

- PolicySets - There's a Policy Sets tab under Governance
  - ACM Hardening
  - OpenShift Hardening
  - OpenShift Plus -- in this demo environment only ACS is deployed from the OpenShift Plus policy set
- Generated Policy Sets - Look at the source content in git either in the [demo repo](https://github.com/gparvin/grc-demo/tree/main/2.5/policy-sets) 
  or in the [policy-collection](https://github.com/stolostron/policy-collection/tree/main/policygenerator/policy-sets)
- Secured Hub Templates - Make sure you deployed an extra managed cluster!  
  - Select the policy `policy-advanced-managed-cluster-security`
  - Select the `Results` tab
  - Select the `View details` link
  - Scroll down in the `Template yaml` panel and you will see the encrypted certificate data that the ACS Sensors need.
- Policy Generator - the sample provided with the generator is installed.  See the details at [policy-collection](https://github.com/stolostron/policy-collection/tree/main/policygenerator/kustomize)
- Configuration Policy custom Evaluation Intervals
  - Look at the policy named `policy-install-kyverno`
  - Navigate to `Results` then select the `View details` link for the `policy-kyverno-namespace` template
  - Note the custom interval in the Template yaml panel
- Policy Enforcement 
  - Many policies are in `inform` mode by default. Check to see if any of these are currently non compliant and enforce to make compliant
  - Gatekeeper operator install
  - Kyverno install
  - MCE and MCH config policies

