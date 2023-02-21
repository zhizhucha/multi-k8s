# Read Me


## Cleaning 

We first want to remove old deployments
```
kubectl get deployments
kube delete deployment client-deployment

kubectl get services
kubectl delete service client-node-port
```

## Apply

```
# We could use : 
kubectl apply -f k8s/client-deployment.yaml 

# But there is  a shortcut that will apply a group of config:
kubectl apply -f k8s
```

## Components

### Client

### Server

### Worker

Nothing in the worker image that needs to be accessible from anything else inside of the cluster. So, multi-worker does not need any port assigned to it.
 
### Redis

### Postgres

### Postgres PVC

## About configuration files

We do a separate configuration file for every object.
Alternativelly, we could regroup the configuration in one file using '---' line to separate.
However, makng separate files clearly tells you where to locate each object configuration, and how many objects are set.


 ## Glossary
**ClusterIP** : Expose *Service* through k8s cluster with ip/name:port. Services are reachable by pods/services in the Cluster. Not from client.

**NodePort** : Expose *Service* through *Internal network VM*. Services are reachable by clients on the same LAN/clients, only good for development env.

**LoadBalancer** : Expose *Service* through *External world*. Services are reachable by everyone connected to the internet. 

**PVC** : Persistent Volume Claim. The same type of volume used in the world of docker to share the file system of a host operating system or a host machine wit the file system inside of a container.

**Volume** :  To have a consistent file system that can be accessed by a database like postgres. Without volume, if postgres crashes, all the data is lost.
In the world of kubernetes, volume is a reference to a very particular type of object. We can write a configuration file. In k8s, this is an object that allows a container to store some persistend data at the pod level.
In addition of volumes, we also have 2 other types of data storage mechanisms: *PersistentVolumeClaim* and *PersistentVolume*. 

**PersistentVolume** : A volume not tied to a container nor a pod. So if a pod for some reason crashed, the data is not lost.

**PersistentVolumeClaim** : Compared to a *PersistentVolume*. Its an advertissement. Its an option that should be available.
Maybe be *statiscally provisioned persistent volume* ( = very specifically created ahead of time), or maybe *dynamically provisioned, persistent volume* (on the fly). 

**Secrets** : Securely store a piece of information in the cluster, such as a database password.
- Use an imperative command to create a secret such as: 
```
# From literal -> as opposed to from file
kuberctl create secret generic <secret_name> --from-literal key=value
(PGPASSWORD=12345)
```

**LoadBalancer** : Legacy way of getting network traffic into a cluster. See *Ingress* for newer way.
Why deprecated: A load balacner service on give acces to a set of pods. In our case we have 2 sets of pods.

**Ingresses** :  A type of service that is going to enventually get some amount of traffic into our application.
In the world of k8s, there is several ingress implementation. We are using nginx-ingress (from github.com/kubernetes/ingress-nginx)

**Controller** : Any type of object that constantly work to make some desired state a reality inside a cluster. Ex: Deployment is a type of *Controller*: As it constantly works to make sure these routing rules are setup.
An *Ingress Controller* : Using nginx behind the scene, we feed in the config file,  the controller is going to create a pod running NGINX that's gonna have a very particular set of rules to make sure that traffic comes in and gets sent off to the appropriate different services inside of our cluster.

```mermaid
graph TD;
    subgraph A["Node"]
        conf["Ingress Config"] --> cont["Ingress Controler"];
        cont --> som["Something that accepts incoming traffic"];
        som --> s1["Service"];
        som --> s2["Service"];
    end


```


```mermaid
graph TD;
    subgraph A["PostgresDeployment"]
        subgraph Pod
            subgraph B["PostgresContainer"]
                data_0["data"];
                data_k["data"];
                data_n["data"];
            end
        end
    end
    node["Request to write Data"]<-->data_0;
```

```mermaid
    graph TD;
        OtherObjInCluster--> Port5000;
        Port5000--> ClusterIPService;

```

### Retrieve minikube ip
```
minikube ip
```


### Deployement in Prod


```mermaid
    flowchart TD
        id1["Create Github Repo."] --> id2["Tie repo to Travis CI"]
        id2 --> id3["Create Google Cloud project"]
        id3 --> id4["Enable billing for the project"]
        id4 --> id5["Add deployment scripts to the repo"]

```
