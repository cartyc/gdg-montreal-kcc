# GDG Montreal "Rise Of Config Connector" Demo

In this demonstration repo we will deploy a GKE Standard cluster and a GKE Autopilot cluster to a GCP project using [Kubernetes Config Connector](https://cloud.google.com/config-connector/docs/overview) and [Config Controller](https://cloud.google.com/anthos-config-management/docs/concepts/config-controller-overview).

This guide assumes you have access to a Google Cloud and can create new projects with an attached billing id and you will be running the commands from Cloud Shell. If you're running things locally you'll need the following tools
- [kpt](https://kpt.dev/installation/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [gcloud cli](https://cloud.google.com/sdk/docs/install)

## Create a Config Controller Instance.

First off we will need to create the config controller instance in the project of your choice.

### Create the Network.

If the `compute.googleapis.com` api has not been enabled previously you will be prompted to enable it. Type `y` and press enter when prompted to do so. You can also follow the [official guide](https://cloud.google.com/anthos-config-management/docs/how-to/config-controller-setup) for this step.
```
gcloud compute networks create default --subnet-mode=auto
```

### Enable APIs
```
gcloud services enable krmapihosting.googleapis.com \
    container.googleapis.com \
    cloudresourcemanager.googleapis.com
```

### Create Config Controller
```
gcloud anthos config controller create gdg-demo \
    --location=us-east1
```

### Authenticate with the Instance

```
gcloud anthos config controller get-credentials gdg-demo \
    --location northamerica-northeast1
```

### Grant Permissions

This will grant owner permissions for the project for east of use but in production you will most likely want to grant a limited set of permissions. 

```
export SA_EMAIL="$(kubectl get ConfigConnectorContext -n config-control \
    -o jsonpath='{.items[0].spec.googleServiceAccount}' 2> /dev/null)"
gcloud projects add-iam-policy-binding "${PROJECT_ID}" \
    --member "serviceAccount:${SA_EMAIL}" \
    --role "roles/owner" \
    --project "${PROJECT_ID}"
```

## Create the GKE Standard Cluster

Pull the package using `kpt`

```
kpt pkg get git@github.com:cartyc/gdg-montreal-kcc.git/gke-standard@main gke-standard
```

1. Move into the newly created directory
```
cd gke-standard
```

2. Initialize the `kpt` inventory
```
kpt live init --namespace config-control
```

3. Edit the setters.yaml file with the appropriate values.


4. Render the changes
```
kpt fn render
```

5. Apply the configurations to the Config Controller Instance.
```
kpt live apply
```

6. Move back up a directory to start the autopilot cluster install.

```
cd ..
```

## Create the GKE Autopilot Cluster

1. Pull the package using `kpt`

```
kpt pkg get git@github.com:cartyc/gdg-montreal-kcc.git/gke-autopilot@main gke-autopilot
```

2. Move into the newly created directory
```
cd gke-autopilot
```

3. Initialize the `kpt` inventory
```
kpt live init --namespace config-control
```

4. Edit the setters.yaml file with the appropriate values.

5. Render the changes

```
kpt fn render
```

6. Apply the configurations to the Config Controller Instance.
```
kpt live apply
```
