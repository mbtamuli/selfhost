# Self Host using Kubernetes

For now, I'm using a local kind cluster, hoping that I can run it on any other k8s cluster as well.

## Prerequisites

* kind
* kubectl
* docker
* docker compose

## Setup

1. Create a kind cluster
    ```sh
    kind create cluster --config kind-config.yaml
    ```
2. Start the `cloud-provider-kind`
    ```sh
    docker compose up -d
    ``` 
3. Setup n8n
    ```sh
    # This command failed for me the first time, because 
    # the namespace `n8n` did not exist while some 
    # resources were being created. Just re-run.
    kubectl apply --files n8n
    ```
4. Check the pods are running
    ```console
    $ kubectl config set-context --current --namespace=n8n
    $ kubectl get pods
    NAME                        READY   STATUS    RESTARTS       AGE
    n8n-7cddd98d7c-jmwtx        1/1     Running   0              3m48s
    postgres-57dbbcbb58-dlgdr   1/1     Running   1 (3m7s ago)   3m54s
    ```
4. Check the IP of the n8n service
    ```console
    $ kubectl get service/n8n
    NAME   TYPE           CLUSTER-IP      EXTERNAL-IP     PORT(S)          AGE
    n8n    LoadBalancer   10.96.119.192   192.168.107.6   5678:30185/TCP   4m59s
    ```
5. Access n8n using the External IP from the above command.
    ```console
    http://192.168.107.6:5678
    ```
