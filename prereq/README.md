# Prerequisites

You will need each of the following in order to complete the workshop:

1. Kubernetes (installed locally or have remote access to a cluster)
1. Meshplay (installed locally)
1. Istioctl (installed locally)

## Create a Kubernetes Cluster<a name="1"></a>

You will need  access to a Kubernetes cluster in this training. While any Kubernetes cluster _should_ work, instructions for Docker Desktop and Minikube are included in these labs as the example Kubernetes platforms. Alternatively, you may choose to use any of the other [supported Kubernetes platform](https://github.com/khulnasoft/meshplay#run-meshplay).

### Setup Docker Desktop (MacOS and Windows)

1. Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop).
   1. Ensure 4GB is allocated to your Docker Desktop VM in Docker Desktop preferences ([see screenshot](https://raw.githubusercontent.com/khulnasoft/istio-service-mesh-workshop/master/prereq/img/docker-desktop-memory.png)).
1. Create Kubernetes cluster. Enable Kubernetes in Docker Desktop preferences ([see screenshot](https://raw.githubusercontent.com/khulnasoft/istio-service-mesh-workshop/master/prereq/img/docker-desktop-kube.png)).
1. Please open `~/.kube/config` and check the `docker-desktop` cluster under `clusters` section and ensure you see something like the image below:
   ![](img/docker-desktop-config.png)

   **Note**: If you see `https://localhost:6443` as the value for server, please get the IP address of your host and replace `localhost` with the the IP address. The end result should look like this `https://1.2.3.4:6443`.

- Mac and Windows users may continue this workshop with Kubernetes on Docker Desktop.

### Or... Setup Minikube (MacOS, Windows, Linux)

1. [Install minikube](https://minikube.sigs.k8s.io).
1. Create Kubernetes cluster: `minikube start`.

### Check Cluster Status

Check the status of the nodes. Ensure `Ready` state.

```sh
[node1 ~]$ kubectl get nodes
NAME      STATUS    ROLES     AGE       VERSION
node1     Ready     master    1h        v1.15.2
```

Check the status of the pods next:

```sh
[node1 ~]$ kubectl get pods --all-namespaces
NAMESPACE     NAME                            READY     STATUS    RESTARTS   AGE
kube-system   etcd-node1                      1/1       Running   0          1h
kube-system   kube-apiserver-node1            1/1       Running   0          1h
kube-system   kube-controller-manager-node1   1/1       Running   0          1h
kube-system   kube-dns-545bc4bfd4-nnbwn       3/3       Running   0          1h
kube-system   kube-proxy-pxq27                1/1       Running   0          1h
kube-system   kube-scheduler-node1            1/1       Running   0          1h
```

If all pods are in a `Running` state, you have an operational Kubernetes cluster. Please continue to download and run Meshplay.

## Download `meshplayctl`<a name="3"></a>

### Meshplay Architecture
In this workshop, Meshplay and Istio adapter for Meshplay will be running in-cluster in the `meshplay` namespace.

<img src="img/meshplay-architecture.svg" alt="Meshplay Architecture" style="float: left; margin-right: 10px;" width="60%" />



#### Install on MacOS and Linux with bash script:
The below command installs Meshplay and Istio adapter for Meshplay in `meshplay` namespace in your cluster.
```
curl -L https://meshplay.io/install | ADAPTERS=istio PLATFORM=kubernetes bash -
```

#### Or.... Install on Windows with `meshplayctl` binary

### [Windows](https://meshplay.khulnasoft.com/docs/installation#windows)

1. Use Scoop.

or

1. Download and unzip `meshplayctl` from the [Meshplay releases](https://github.com/khulnasoft/meshplay/releases/latest) page.
1. Add `meshplayctl` to your PATH for ease of use. Then, execute:

```
./meshplayctl system start
```

Upon starting Meshplay successfully, instructions to access Meshplay will be printed on the sceen.

## Run Meshplay

Meshplay will automatically launch in your browser.

Sign into Meshplay ([see screenshot](/master/prereq/img/sign-into-meshplay.png)) using either Twitter, Linkedin, GitHub or Google authentication.

Meshplay attempts to automatically connect with your Kubernetes cluster by loading the kubeconfig found in your `$HOME/.kube` folder and connecting existing service mesh adapters ([see screenshot](../master/prereq/img/meshplay_landing_page.png)).

If your kubeconfig is in a different location (i.e. if you are not using Docker Desktop), point Meshplay to your kubeconfig location by navigating to the Settings page. Navigate to Settings by clicking the gear icon on the right top of the screen ([see screenshot](https://raw.githubusercontent.com/khulnasoft/advanced-istio-service-mesh-workshop/master/prereq/img/meshplay_landing_page_settings_icon.png)).

This will take the user to the `Settings` page and here you can load up your new config file and select the context to use ([see screenshot](https://raw.githubusercontent.com/khulnasoft/advanced-istio-service-mesh-workshop/feature/blend-in-meshplay/prereq/img/meshplay_settings_page.png)).

**If you are using minikube**:
To configure Meshplay to use minikube:

1. Login to Meshplay. Under your user profile, click `Get Token`.
1. Use `meshplayctl` to configure Meshplay to use minikube. Execute:

```sh
meshplayctl system config minikube -t ~/Downloads/auth.json
```

In a similar fashion, if you don't see the Istio adapter loaded, you should be able to switch to the `Service Meshes` tab in the `Settings` page and connect to existing adapters from the drop down ([see screenshot](https://raw.githubusercontent.com/khulnasoft/advanced-istio-service-mesh-workshop/master/prereq/img/meshplay_settings_page-service-meshes.png)).

Once an adapter is connected, you will also see it added to the nav menu on the left ([see screenshot](https://raw.githubusercontent.com/khulnasoft/advanced-istio-service-mesh-workshop/master/prereq/img/meshplay_settings_page-service_meshes_with_menu.png)).

In the labs, you will use a combination of Meshplay's UI and your terminal. We suggest splitting the view on your display between your terminal and your web browser, so that you don't have to switch between apps frequently.

<h2>
  <a href="../lab-1/README.md">
  <img src="../img/go.svg" width="32" height="32" align="left" />
  Continue to Lab 1</a>: Deploy Istio
</h2>

<br />
<hr />

Optional, manual installation steps are provided for reference below. No need to execute these if you have performed the steps above.

<hr />

## Download `istioctl`<a name="4"></a>

**Bash**
Install `istioctl` CLI on your local system by executing:

```sh
curl -L https://istio.io/downloadIstio | sh -
export PATH=$PWD/bin:$PATH
```

**Brew**
If you are on MacOS, you can use homebrew to install Istio cli:

```sh
brew install istioctl
```

To verify if the cli was successfully installed:

```sh
istioctl version
```
