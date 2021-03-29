# Controllers

Defines the desired state of cluster

Create and manage pods

Respond to pod state and health

## Examples

`ReplicaSet` 
- Number of replicated pods in the cluster
- Generally not created directly, but rather as a part of a `Deployment`

`Deployment`
- Manages the state of `ReplicaSets`
- Manages the transition of an application version change