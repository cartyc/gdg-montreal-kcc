# gke-autopilot

## Description
KCC Deployment of a Private GKE Autopilot Cluster

## Usage

### Fetch the package
`kpt pkg get git@github.com:cartyc/gdg-montreal-kcc.git/gke-autopilot{@VERSION} gke-autopilot`
Details: https://kpt.dev/reference/cli/pkg/get/

### View package content
`kpt pkg tree gke-autopilot`
Details: https://kpt.dev/reference/cli/pkg/tree/

### Apply the package
```
kpt live init gke-autopilot
kpt live apply gke-autopilot --reconcile-timeout=2m --output=table
```
Details: https://kpt.dev/reference/cli/live/
