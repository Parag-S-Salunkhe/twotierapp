## HELM, package manager

**What is a Package Manager?**

A package manager is a tool that automates the process of installing, upgrading, configuring, and removing software packages. These packages contain all the necessary files, metadata, and dependencies required for the software to function properly. Package managers simplify software management by handling the complex dependencies and configurations that can arise when multiple packages are installed on a system.

------------------------------------------------------------------------
**Examples of Package Managers**

APT (Advanced Package Tool): Used in Debian-based systems like Ubuntu.

Yum/DNF: Used in Red Hat-based systems like CentOS and Fedora.

Homebrew: Used on macOS.

npm: Used for JavaScript and Node.js applications.

pip: Used for Python packages.

------------------------------------------------------------------------
**Why is Helm Called a Package Manager?**

Helm is called a package manager for Kubernetes because it performs similar functions to traditional package managers but for Kubernetes applications. It manages Kubernetes resources through a concept called "charts," which are packages of pre-configured Kubernetes resources.

------------------------------------------------------------------------
**Key Features of Helm**

Packaging: Helm packages Kubernetes resources into charts. A chart is a collection of files that describe a related set of Kubernetes resources.

Dependency Management: Helm charts can specify dependencies on other charts. Helm manages these dependencies, ensuring that all necessary components are installed correctly.

Versioning: Helm supports versioning of charts, allowing you to track and manage different versions of your applications.

Configuration: Helm allows you to configure charts through a values.yaml file, which can override default settings in a chart.

Installation and Upgrades: Helm simplifies the installation and upgrading of Kubernetes applications. With a single command, Helm can install or upgrade an application, handling the underlying Kubernetes resources automatically.

Rollbacks: Helm supports rollbacks to previous versions of a release, making it easier to recover from failed deployments.

-------------------------------------------------------------------------
**How Helm Works**

Charts: Helm uses charts to define Kubernetes applications. Each chart contains a set of templates, a default values.yaml file, and other metadata.

Releases: When you install a chart, Helm creates a release. A release is a specific instance of a chart running in a Kubernetes cluster.

Repositories: Helm charts are stored in repositories. Helm can fetch and install charts from these repositories.

## pre-requisites and installation

Kubernetes cluster set up (e.g., Minikube, kind, or any cloud-based Kubernetes)
Docker installed (Optional for custom images)

installation :
```
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

## Important Helm Commands

- `helm create [CHART]`: Scaffold a new Helm chart.
- `helm package [CHART]`: Package the chart into a chart archive.
- `helm install [NAME] [CHART]`: Install a Helm chart.
- `helm upgrade [NAME] [CHART]`: Upgrade an installed Helm chart.
- `helm uninstall [NAME]`: Uninstall an installed Helm chart.
- `helm list`: List all installed Helm charts.
- `helm rollback [NAME] [REVISION]`: Roll back a release to a specific revision.

---------------------------------------------------------------------------------------
## Implementing helm
By default helm creates nginx tempelate, which has 2 folders and 2 files (one package). for this project we will work on values.yml and in template folder we create the deployment in template. We will make a package of sql and flask app.

```
heml create name-chart
```
as we don't want to hardcode the values of the environments, we will use values.yml to inject them into deployment.yml of service, this helps us change database and password from values file making the app more secure.

follow [sequel chart](https://github.com/Parag-S-Salunkhe/twotierapp/tree/helm/mysql-chart)

afer the charts are ready we package them for installation

```
helm package chart-name
helm install chartname ./mysql-chart
helm list
helm unistall chart name ## if you need to uninstall
kubectl get all ## gets all pods, services, deployments
```

create chart for flask app, follow [flask-chart](https://github.com/Parag-S-Salunkhe/twotierapp/tree/helm/flask-app-chart)

package it and install.

```
helm template chartname  ## to see all the injections
```
