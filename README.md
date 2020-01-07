# cloud-native-handson

## step 1

Build a `docker` image to containerize your java application (`build/libs/cn-worskshop.jar`).

_Run it locally for testing:

    docker run --rm -it -p 8080:8080 cn-workshop1/greeting-service-X:v1

_Push it into DockerHub using following user (`docker login docker.io`):

    username: <REGISTRY_USER>
    password: <REGISTRY_PWD>

## step 2

To use the full power of Kubernetes as a declarative infrastructure as code system, you can submit YAML manifests to the
cluster yourself, using `kubectl` commands. 

_Adjust following `deployment` configuration:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo
  labels:
    app: demo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: demo
  template:
    metadata:
      labels:
        app: demo
    spec:
      containers:
        - name: demo
          image: <REGISTRY_USER>/demo:latest
          ports:
          - containerPort: 8888
```

Apply it to the cluster in your namespace:

    kubectl create namespace <namespace-name>
    kubectl create -f <deployment-yaml-file> -n <namespace-name>

Connect the `pod` to a port on your local machine, so that you can call it with your browser or curl command:

    kubectl port-forward <pod-name> <kubernetes-port>:<local-port> -n <namespace-name>
    curl localhost:<local-port>/hello

## step 3

One popular package manager for Kubernetes is called `Helm`, and you can use its command-line tool to install and 
configure applications (your own or anyone else's), and you can create packages called Helm `charts`, which completely 
specify the resources needed to run the application, its dependencies, and its configurable settings.

When you install a Helm chart, Kubernetes itself will locate and download the binary container image from the place you 
specified. In fact, a Helm chart is really just a convenient wrapper around Kubernetes YAML manifests.

_create new chart directory `demo-helm` with following files:

demo-helm/templates/deployment.yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Values.container.name }}
spec:
  replicas: {{ .Values.replicas }}
  selector:
    matchLabels:
      app: {{ .Values.container.name }}
  template:
    metadata:
      labels:
        app: {{ .Values.container.name }}
    spec:
      containers:
        - name: {{ .Values.container.name }}
          image: {{ .Values.container.image }}:{{ .Values.container.tag }}
          ports:
              - containerPort: {{ .Values.container.port }}
```

demo-helm/Chart.yaml
```yaml
apiVersion: v2
name: demo-helm
description: A Helm chart for Kubernetes
type: application
version: 0.1.0
appVersion: 1.0.0
```

demo-helm/values.yaml
```yaml
container:
  name: demo
  port: 8888
  image: <REGISTRY_USER>/demo
  tag: latest
replicas: 1
```

_create an alias for helm-cli based on docker image:

    alias helm3='docker run --rm -it -v $(pwd):/apps -v ~/.kube:/root/.kube alpine/helm:3.0.1'

_test the manifest with:

    helm3 template demo-helm

_install the chart (after deleting the old `deployment` first!) with:

    helm3 install <release-name> ./demo-helm

## step 4

An app’s config is everything that is likely to vary between deploys (staging, production, developer environments, etc).
Apps sometimes store config as constants in the code. This is a violation of twelve-factor, which requires strict 
separation of config from code. Config varies substantially across deploys, code does not.

The twelve-factor app stores config in environment variables (often shortened to env vars or env). Env vars are easy to 
change between deploys without changing any code; unlike config files, there is little chance of them being checked into 
the code repo accidentally; and unlike custom config files, or other config mechanisms such as Java System Properties, 
they are a language- and OS-agnostic standard.

_add a `ConfigMap` to the helm chart (in `templates` folder):

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Values.container.name }}-config
data:
  greeting-key: Hola
```

_update `deployment.yaml` template by adding config-map reference (underneath `container`):

```yaml
envFrom:
- configMapRef:
    name: {{ .Values.container.name }}-config
```

_update `HelloController.java` to return the env variable `GREETING`:

```java
@Value("${MY_ENV_VAR:default-value}")
private String foo;
```

_build new `docker image` and update the `helm chart`. Upgrade your cluster deployment by calling:

    helm3 upgrade <release-name> ./demo-helm

_validate the changes using `kubectl port-forward` command

## step 5

Not every bug in your code can be traced by adding `log.debug()` statements. Attaching a debugger to your running code
can sometimes magically help you to see what’s wrong with your code.

_update `configmap.yaml` to automatically apply all key values from `values.yaml`

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Values.container.name }}-config
data:
  {{- toYaml .Values.data | nindent 2 }}
```

_add `GREETING` & `JAVA_TOOL_OPTIONS` env variable (in the `values.yaml`) to allow remote debugging on port `5005`:

```yaml
data:
  GREETING: hola
  JAVA_TOOL_OPTIONS: -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005
```

_extend `Dockerfile` to expose port `5005`

_rebuild & redeploy updated `docker image`

_forward the pod's port `5005` into your local machine and attach your IDE's remote configuration 
`Run > Edit Configurations > Remote` into `localhost:5005`

## step 6

_install `Tilt` from https://docs.tilt.dev/install.html

_add `Tiltfile` at repo's root:

```yaml
# docker tag, build context
docker_build('<REGISTRY_USER>/demo', '.')
# path to helm chart
k8s_yaml(helm('demo-helm'))
# port-forward on 'cn-workshop' resource
k8s_resource('demo', port_forwards=8080)
# allow GKE cluster
allow_k8s_contexts('gke_hypnotic-surge-215419_europe-west1-b_cn-workshop')
```

_adjust helm's `values.yaml` file updating `container.image` value with repository prefix

_use `tilt up` to start the development mode








TODO BB:

- add readiness-step:
  - add service
  - add readiness-probe on /actuator/health
  - port-forward on service showing value 1,1,1,2,2,2,2