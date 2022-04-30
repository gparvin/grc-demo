# ACM 2.5 Demo Setup

Run these commands to apply the ACM 2.5 features.

```
oc create ns policies
cd deploy
./deploy.sh -u https://github.com/stolostron/grc-demo.git -p 2.5/policy-sets/acm-hardening -n policies -a acm
./deploy.sh -u https://github.com/stolostron/grc-demo.git -p 2.5/policy-sets/openshift-hardening -n policies -a ocp
./deploy.sh -u https://github.com/stolostron/grc-demo.git -p 2.5/policy-sets/openshift-plus -n policies -a acs
./deploy.sh -u https://github.com/stolostron/grc-demo.git -p 2.5/policy-sets/kustomize -n policies -a sample
./deploy.sh -u https://github.com/stolostron/grc-demo.git -p 2.5/policies -n policies -a policies
```
