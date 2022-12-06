# ACM 2.7 Demo Setup

Here's a quick procedure to get setup with our ACM 2.7 features.

## Deploy Policies

Use the deploy script in the `deploy` subdirectory to deploy policies and policy sets.  The commands below
install our sample policy sets, our sample generator created policies and a few extra policies that all
display different features in GRC.

```
cd deploy
echo y | ./deploy.sh -u https://github.com/gparvin/grc-demo.git -p 2.7/policy-sets/openshift-plus-setup -n policies -a acm
echo y | ./deploy.sh -u https://github.com/gparvin/grc-demo.git -p 2.7/policy-sets/acm-hardening -n policies -a acm
echo y | ./deploy.sh -u https://github.com/gparvin/grc-demo.git -p 2.7/policy-sets/openshift-hardening -n policies -a ocp
echo y | ./deploy.sh -u https://github.com/gparvin/grc-demo.git -p 2.7/policy-sets/openshift-plus -n policies -a acs
echo y | ./deploy.sh -u https://github.com/gparvin/grc-demo.git -p 2.7/policy-sets/kustomize -n policies -a sample
echo y | ./deploy.sh -u https://github.com/gparvin/grc-demo.git -p 2.7/policies -n policies -a policies
```

## Add a managed cluster

To fully show some functionality we need to create a managed cluster.  It's quick and easy to create an AWS credential
and then a new cluster.  The OpenShift Plus PolicySet that install ACS uses the secure hub templates only for 
non-hub managed clusters.

## Links to features

Which policies detail which features in ACM?  Here's a list of a feature and you can navigate to that feature with 
these hints:

### ACM 2.7 features

- Automatically update policies when hub template references are changed
- Provide context on policy failure details to Ansible Automation jobs
- Dependencies between policies to control execution order

### ACM 2.6 features

- Namespace selector improvements
  - See sample under `policies/policy-certificate.yaml` which uses a namespace label for certificate checking
- Configurable concurrency - nothing to demo. Change is to the ManagedClusterAddon for the config-policy-controller
- Ansible integration for `everyEvent` mode
  - Best to show this is the UI for an environment configured for Ansible Automation
- Deleting resources managed by a policy when the policy is deleted
  - See sample under `policies/policy-role.yaml` which will create an RBAC role when deployed and will delete it when the policy is deleted

### ACM 2.5 features

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

