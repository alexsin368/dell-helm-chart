# Enterprise Inference Helm Chart

This Helm chart deploys Enterprise Inference, an open-source LLM serving stack built for Intel® Xeon® processors and Intel® Gaudi® AI Accelerators for cloud and on-premise environments.

## Introduction

TODO

## Prerequisites

TODO

## Installing the Chart

Set the device to deploy on, default is "hpu". Set your Hugging Face token value.
```bash
export DEVICE="hpu" # Options: hpu, cpu
export HUGGINGFACE_TOKEN="<your-hf-token>"
```

### From the Helm Repository

```bash
# Add the repository
helm repo add deh https://huggingface.github.io/dell-helm-chart
helm repo update

# Install the chart
helm install enterprise-inference deh/enterpriseinference \
  --set device=$DEVICE \
  --set main.config.storageClassName=gp2 \
  --set main.config.clusterUrl="https://inference.dell.local" \
  --set main.config.models=["1"] \
  --set main.config.cpuOrGpu="gaudi3" \
  --set main.secrets.huggingFaceToken=$HUGGINGFACE_TOKEN
```

### From Local Source

```bash
# Clone the repository
git clone https://github.com/huggingface/dell-helm-chart.git
cd dell-helm-chart

# Install the chart
helm install enterprise-inference ./charts/apps/enterpriseinference \
  --set device=$DEVICE \
  --set main.config.storageClassName=gp2 \
  --set main.config.clusterUrl="https://inference.dell.local" \
  --set main.config.models=["1"] \
  --set main.config.cpuOrGpu="gaudi3" \
  --set main.secrets.huggingFaceToken=$HUGGINGFACE_TOKEN
```


## Configuration

The following table lists the configurable parameters for the Enterprise Inference chart:

### Global Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `nameOverride` | Override the name of the chart | `""` |
| `fullnameOverride` | Override the fullname of the chart | `""` |
| `device` | Set the target device to deploy models on| `hpu` |

### Main Component Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `main.enabled` | Enable the main EnterpriseInference component | `true` |
| `main.image.pullPolicy` | OpenWebUI image pull policy | `IfNotPresent` |
| `main.config.storageClassName` | Storage class for persistent data (required) | `gp2` |
| `main.config.clusterUrl` | The base URL to host the inference endpoint | `https://inference.dell.local` |
| `main.config.models` | A list of the model numbers from the list of pre-validated models | `[1]` |
| `main.config.cpuOrGpu` | The target platform to deploy the model | `gaudi3` |
| `main.secrets.huggingFaceToken` | Hugging Face token to access models | `hf_xxxxx` |
| `main.service.type` | Service type for the main component | `ClusterIP` |
| `main.service.port` | Service port for the main component | `8080` |
| `main.resources` | Resource requests and limits for the main component | modest requests/limits |
| `main.persistence.enabled` | Enable persistence for the main component | `true` |
| `main.persistence.size` | Size of the persistent volume claim | `1Gi` |
| `main.securityContext.enabled` | Enable security context settings | `true` |
| `main.securityContext.fsGroup` | The group ID for volume permissions | `0` |
| `main.securityContext.runAsUser` | The user ID to run the container processes | `0` |
| `main.securityContext.runAsGroup` | The group ID to run the container processes | `0` |
| `main.securityContext.runAsNonRoot` | Require the container to run as a non-root user | `false` |


## Features

- **Model Serving & Compatibility**: Deploy and run a wide range of foundation models including LLaMA, Mistral, Whisper, Stable Diffusion, and BYOM (Bring Your Own Model). Supports versioning and rollback for reproducible model lifecycle management.
- **Inference Engine**: Built on vLLM for optimized memory usage, dynamic batching, and concurrent request handling, enabling low-latency and high-throughput inference across diverse workloads.
- **Intel Gaudi Base Operator**: A specialized operator that manages the lifecycle of Habana AI resources within the Kubernetes cluster, enabling efficient utilization of Intel® Gaudi® hardware for AI workloads (applicable only to Gaudi-based deployments).
- **Infrastructure-Aware Scheduling**: Automatically leverages available hardware for performance optimization:
  - Intel Xeon for lightweight inference and low-latency tasks.
  - Intel Gaudi for multi-model scaling and high-throughput deployments.
- **API Access Management**: Utilizes Keycloak, an open-source identity and access management solution that provides robust authentication and authorization capabilities, ensuring secure access to AI services and resources within the cluster.
- **API Gateway & Traffic Management**: APISIX API gateway for request routing, load balancing, rate limiting, and quota enforcement. Ingress controller in Kubernetes handles external traffic, TLS termination, and secure endpoint exposure.
- **Deployment Assets & Extensibility**: Fully open-source assets on GitHub with Helm charts, Terraform templates, and Dockerfiles. Integrates into existing CI/CD pipelines and supports extension with custom APIs or services.
- **Observability & Monitoring**: Native integration with Prometheus and Grafana providing comprehensive visibility into the performance, health, and resource utilization of deployed applications and cluster components through metrics, visualization, and alerting capabilities.
- **Scalable Architecture**: Container-native design supporting Kubernetes deployments with autoscaling, ingress management, and model sharding across nodes.
- **Security & Governance**: Built-in authentication support by Keycloak, and API-level policy enforcement via APISIX, with audit logging to ensure secure and compliant inference workloads.


## Additional Resources

- [Enterprise Inference GitHub Repository](https://github.com/opea-project/Enterprise-Inference)
- [vLLM with Intel® Gaudi® AI Accelerators Documentation](https://github.com/HabanaAI/vllm-fork/blob/habana_main/README_GAUDI.md) 