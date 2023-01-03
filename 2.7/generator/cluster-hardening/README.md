# PolicySets -- Kuernetes Cluster Hardening

See the [Policy Generator](https://github.com/stolostron/policy-generator-plugin) 
Kustomize plugin for more information on using the policy generator.

## PolicySet details

The Kubernetes Cluster Hardening `PolicySet` Applies best practices for how to make sure your cluster is 
configured properly and is healthy.
This `PolicySet` can be deployed to any kubernetes cluster.

**Note**: The `PolicySet` uses cluster `Placement` and not the `PlacementRule` placement mechanism.
