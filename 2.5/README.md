# ACM 2.5 Demo Setup

Run these commands to apply the ACM 2.5 features.

```
oc create ns policies
cd deploy
echo y | ./deploy.sh -u https://github.com/gparvin/grc-demo.git -p 2.5/policy-sets/acm-hardening -n policies -a acm
echo y | ./deploy.sh -u https://github.com/gparvin/grc-demo.git -p 2.5/policy-sets/openshift-hardening -n policies -a ocp
echo y | ./deploy.sh -u https://github.com/gparvin/grc-demo.git -p 2.5/policy-sets/openshift-plus -n policies -a acs
echo y | ./deploy.sh -u https://github.com/gparvin/grc-demo.git -p 2.5/policy-sets/kustomize -n policies -a sample
echo y | ./deploy.sh -u https://github.com/gparvin/grc-demo.git -p 2.5/policies -n policies -a policies
```
