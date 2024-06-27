# HTTP Proxy Helm Chart for Oracle PCA OKE

This repository contains a Helm chart for deploying an HTTP proxy configuration in the Oracle Kubernetes Engine (OKE) of the Oracle PCA appliance.
This HTTP proxy allows pulling images from the internet. 
Optionally, the proxy configuration can be automatically injected into every container deployed by the customer.

## Prerequisites

- Oracle PCA appliance with OKE
- Helm 3.x

## Installation

To install the chart with the release name `my-proxy`:

```sh
helm install my-proxy ./path/to/helm-chart --set image=<image> --set httpProxy=<http-proxy-endpoint> --set httpsProxy=<https-proxy-endpoint> --set noProxy=<no-proxy-variable>
```

## Parameters

The following table lists the configurable parameters of the chart and their default values.

|        Parameter        |                        Description                        |                                                    Default                                                   |
|:-----------------------:|:---------------------------------------------------------:|:------------------------------------------------------------------------------------------------------------:|
| image.repository        | The container image to use it must have systemctl and sh  | regionregistry.pca.local/oracle/cloud-provider-oci                                                           |
| proxy.http              | The HTTP proxy endpoint                                   | http://proxy.local:8080                                                                                      |
| proxy.https             | The HTTPS proxy endpoint                                  | http://proxy.local:8080                                                                                      |
| proxy.no_proxy          | Comma-separated list of domains to bypass the proxy       | *.oraclevcn.com, *.local, localhost, 127.0.0.1,10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16, 169.254.169.254/32 |
| proxy.auto_inject_proxy | Automatically inject PROXY environment to every container | false                                                                                                        |


```sh
helm install my-proxy ./path/to/helm-chart --set image=my-custom-image --set httpProxy=http://proxy.example.com:8080 --set httpsProxy=https://proxy.example.com:8443 --set noProxy=localhost,127.0.0.1,.example.com
```


## values.yaml
```yaml
image:
  repository: regionregistry.pca.local/oracle/cloud-provider-oci
  pullPolicy: IfNotPresent
  tag: "v1.26.2"

proxy:
  http: http://proxy.local:8080
  https: https://proxy.local:8080
  no_proxy: "*.oraclevcn.com,.*.local,localhost,127.0.0.1,10.0.0.0/8,172.16.0.0/12,192.168.0.0/16,169.254.169.254/32"
  auto_inject_proxy: false
```
Then install the chart with the custom values:

```sh

helm install my-proxy ./path/to/helm-chart -f values.yaml
```

## Contributing

We welcome contributions to enhance this Helm chart! Please fork the repository and submit a pull request.