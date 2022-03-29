# gke-standard

## Description
sample description

## Usage

### Fetch the package
`kpt pkg get git@github.com:cartyc/gdg-montreal-kcc.git/gke-standard{@VERSION} gke-standard`
Details: https://kpt.dev/reference/cli/pkg/get/

### View package content
`kpt pkg tree gke-standard`
Details: https://kpt.dev/reference/cli/pkg/tree/

### Apply the package
```
kpt live init gke-standard
kpt live apply gke-standard --reconcile-timeout=2m --output=table
```
Details: https://kpt.dev/reference/cli/live/
