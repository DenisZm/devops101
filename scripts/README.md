# Kubeplugin - Kubernetes Resource Usage Plugin

This plugin shows CPU and Memory usage statistics for Kubernetes resources.

## Features

- Display CPU and Memory usage statistics for pods
- Display CPU and Memory usage statistics for nodes
- Support for different namespaces
- Works as kubectl plugin or standalone script

## Installation as kubectl plugin

### 1. Rename the file
```bash
mv kubeplugin kubectl-kubeplugin
```

### 2. Make it executable
```bash
chmod +x kubectl-kubeplugin
```

### 3. Move to a directory in PATH
Option A - system directory:
```bash
sudo mv kubectl-kubeplugin /usr/local/bin/
```

Option B - add current directory to PATH:
```bash
export PATH=$PATH:$(pwd)
```

### 4. Verify installation
```bash
kubectl plugin list
```

You should see `kubeplugin` in the list of available plugins.

## Usage

### As kubectl plugin

#### Get nodes statistics:
```bash
kubectl kubeplugin nodes
```

#### Get pods statistics in default namespace:
```bash
kubectl kubeplugin pods
```

#### Get pods statistics in specific namespace:
```bash
kubectl kubeplugin pods kube-system
```

### As standalone script

```bash
# Nodes statistics
./kubeplugin nodes

# Pods statistics in default namespace
./kubeplugin pods

# Pods statistics in specific namespace
./kubeplugin pods kube-system
```

## Output Format

The plugin outputs data in CSV format:
```
Resource, Namespace, Name, CPU, Memory
pods, kube-system, coredns-558bd4d5db-xyz, 3m, 15Mi
pods, kube-system, etcd-minikube, 25m, 45Mi
```

## Requirements

- `kubectl` must be installed and configured
- Kubernetes metrics server must be running in the cluster
- Permission to execute `kubectl top` command

## Examples

### Monitor pods in kube-system namespace:
```bash
kubectl kubeplugin pods kube-system
```
Output:
```
Resource, Namespace, Name, CPU, Memory
pods, kube-system, coredns-558bd4d5db-xyz, 3m, 15Mi
pods, kube-system, etcd-minikube, 25m, 45Mi
pods, kube-system, kube-apiserver-minikube, 55m, 250Mi
```

### Monitor nodes:
```bash
kubectl kubeplugin nodes
```
Output:
```
Resource, Namespace, Name, CPU, Memory
nodes, default, minikube, 158m, 1234Mi
```
