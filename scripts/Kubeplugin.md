# kubeplugin

`kubeplugin` is a script to retrieve and display resource usage statistics for Kubernetes resources.

## Installation and Setup

1. **Ensure you have access to a Kubernetes cluster and `kubectl` installed.**

2. **Install Metrics Server in your Kubernetes cluster:**

    ```sh
    kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
    ```

3. **Place the `kubeplugin` script in the `scripts` directory of your repository.**

4. **Make the script executable:**

    ```sh
    chmod +x scripts/kubeplugin
    ```

## Usage

### Running the Script

To run the script, use the following command:

```sh
./scripts/kubeplugin <namespace> <resource_type>
