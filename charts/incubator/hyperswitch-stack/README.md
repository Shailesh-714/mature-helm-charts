<p align="center">
  <img src="https://raw.githubusercontent.com/juspay/hyperswitch/refs/heads/main/docs/imgs/hyperswitch-logo-dark.svg#gh-dark-mode-only" alt="Hyperswitch-Logo" width="40%" />
  <img src="https://raw.githubusercontent.com/juspay/hyperswitch/refs/heads/main/docs/imgs/hyperswitch-logo-light.svg#gh-light-mode-only" alt="Hyperswitch-Logo" width="40%" />
</p>

<h1 align="center">The open-source payments switch</h1>

<div align="center" >
The single API to access payment ecosystems across 130+ countries</div>

<p align="center">
  <a href="#try-a-payment">💳 Try a Payment</a> •
  <a href="#quick-setup">Setup in k8s</a> •
  <a href="https://api-reference.hyperswitch.io/introduction"> API Docs </a>
   <br>
  <a href="#community-contributions">Community and Contributions</a> •
  <a href="#copyright-and-license">Copyright and License</a>
</p>

<p align="center">
  <a href="https://github.com/juspay/hyperswitch/blob/main/LICENSE">
    <img src="https://img.shields.io/github/license/juspay/hyperswitch" />
  </a>
</p>
<p align="center">
  <a href="https://www.linkedin.com/company/hyperswitch/">
    <img src="https://img.shields.io/badge/follow-hyperswitch-blue?logo=linkedin&labelColor=grey"/>
  </a>
  <a href="https://x.com/hyperswitchio">
    <img src="https://img.shields.io/badge/follow-%40hyperswitchio-white?logo=x&labelColor=grey"/>
  </a>
  <a href="https://join.slack.com/t/hyperswitch-io/shared_invite/zt-2jqxmpsbm-WXUENx022HjNEy~Ark7Orw">
    <img src="https://img.shields.io/badge/chat-on_slack-blue?logo=slack&labelColor=grey&color=%233f0e40"/>
  </a>
</p>

<hr>

Hyperswitch is a community-led, open payments switch designed to empower digital businesses by providing fast, reliable, and affordable access to the best payments infrastructure.

Here are the components of Hyperswitch that deliver the whole solution:

* [Hyperswitch Backend](https://github.com/juspay/hyperswitch): Powering Payment Processing

* [SDK (Frontend)](https://github.com/juspay/hyperswitch-web): Simplifying Integration and Powering the UI

* [Control Centre](https://github.com/juspay/hyperswitch-control-center): Managing Operations with Ease

Jump in and contribute to these repositories to help improve and expand Hyperswitch!

<img src="https://raw.githubusercontent.com/juspay/hyperswitch/refs/heads/main/docs/imgs/switch.png" />

<a href="https://app.hyperswitch.io/">
  <h2 id="try-a-payment">💳 Try a Payment</h2>
</a>

To quickly experience the ease of Hyperswitch, sign up on the [Hyperswitch Control Center](https://app.hyperswitch.io/) and try a payment. Once you've completed your first transaction, you’ve successfully made your first payment with Hyperswitch!

<a href="#Quick Setup">
  <h2 id="quick-setup">⚡️ Quick Setup</h2>
</a>

# Deploy on Kubernetes using Helm

This section outlines cloud-provider agnostic deployment steps for easy installation of the Hyperswitch stack on your K8s cluster

## Installation

### Step 1 - Clone repo and Update Configurations

Clone the [hyperswitch-stack](https://github.com/juspay/hyperswitch-helm) repo and start updating the configs

```bash
helm repo add hyperswitch https://juspay.github.io/hyperswitch-helm
helm repo update
```

### Step 2 - Install Hyperswitch

Before installing the service make sure you labels your kubernetes nodes and create a namespace `hyperswitch`
Note: minimum --memory 6000 --cpus 4 needed
```bash
kubectl create namespace hyperswitch
```
Use below command to install hyperswitch services with above configs

```bash
helm install hypers-v1 hyperswitch/hyperswitch-stack -n hyperswitch
```

That's it! Hyperswitch should be up and running on your Cluster  :tada: :tada:

## Post-Deployment Checklist

After deploying the Helm chart, you should verify that everything is working correctly

### App Server

* [ ] &#x20;Check that `hyperswitch_server/health` returns `health is good`

### Control Center

* [ ] &#x20;Verify if you are able to sign in or sign up
* [ ] &#x20;Verify if you are able to [create API key](https://docs.hyperswitch.io/hyperswitch-open-source/account-setup/using-hyperswitch-control-center#user-content-create-an-api-key)
* [ ] &#x20;Verify if you are able to [configure a new payment processor](https://docs.hyperswitch.io/hyperswitch-open-source/account-setup/using-hyperswitch-control-center#add-a-payment-processor)

## 💳 Test a payment

Hyperswitch Control center will mimic the behavior of your checkout page. Please follow below steps to test a payment with the deployed app

### 🔐 Step 1 - Deploy card vault

By default card vault and its dependencies are installed, however you need to create master key, custodian keys and unlock the locker to start saving cards.

<details>
  <summary>
    <b> Step 1: Generating the keys </b>
  </summary>
  <p>
  To generate the master key and the custodian keys use the following command after cloning the repository.
   
    # Generate master key
    git clone https://github.com/juspay/hyperswitch-card-vault.git
    cd hyperswitch-card-vault
    cargo run --bin utils -- master-key
    To generate the JWE and JWS keys run the following commands

    # Generating the private keys
    openssl genrsa -out locker-private-key.pem 2048
    openssl genrsa -out tenant-private-key.pem 2048

    # Generating the public keys
    openssl rsa -in locker-private-key.pem -pubout -out locker-public-key.pem
    openssl rsa -in tenant-private-key.pem -pubout -out tenant-public-key.pem 
</p>
</details>
<details>
  <summary>
    <b> Step 2: Update the keys in deployment </b>
  </summary>
  <p>

    # Update below values in hyperswitch-stack/values.yaml
    # The public key for the locker from locker-public-key.pem
    hyperswitch-app.server.secrets.kms_jwekey_vault_encryption_key: |
      -----BEGIN PUBLIC KEY-----
      ...
      -----END PUBLIC KEY-----
    # The private key for the tenant from tenant-private-key.pem
    hyperswitch-app.server.secrets.kms_jwekey_vault_private_key: |
      -----BEGIN RSA PRIVATE KEY-----
      ...
      -----END RSA PRIVATE KEY-----
    # The private key for the locker from locker-private-key.pem
    hyperswitch-card-vault.server.secrets.locker_private_key: |
      -----BEGIN RSA PRIVATE KEY-----
      ...
      -----END RSA PRIVATE KEY-----
    # The public key for the tenant from tenant-public-key.pem
    hyperswitch-card-vault.server.secrets.tenant_public_key: |
      -----BEGIN PUBLIC KEY-----
      ...
      -----END PUBLIC KEY-----

   </p>
</details>
<details>
  <summary> <b> Step 3: Unlock the locker </b> </summary>
  <p>
  Once the locker is up and running, use the 2 key custodian keys generated earlier securely to unlock the locker for use.
  Go to the respective locker Pod, open its shell and run below cURLs

  The following cURLs are to be used to provide keys

    # temporary turn of saving to history to run the following commands
    unset HISTFILE

    # Add key1, key2 and then decrypt
    curl -X POST -H "Content-Type: application/json" -d '{"key": "<key 1>"}' http://localhost:8080/custodian/key1
    curl -X POST -H "Content-Type: application/json" -d '{"key": "<key 2>"}' http://localhost:8080/custodian/key2
    curl -X POST http://localhost:8080/custodian/decrypt
   
  If the last cURL replies with `Decrypted Successfully`, we are ready to use the locker.
   </p>
</details>

### Step 2 - Make a payment using our Control Center

Use the Hyperswitch Control Center and [make a payment with test card](https://docs.hyperswitch.io/hyperswitch-open-source/account-setup/test-a-payment).

Refer our [postman collection](https://www.postman.com/hyperswitch/workspace/hyperswitch/folder/25176183-0103918c-6611-459b-9faf-354dee8e4437) to try out REST APIs

<a href="#community-contributions">
  <h2 id="community-contributions">✅ Community & Contributions</h2>
</a>

The community and core team are available in [GitHub Discussions](https://github.com/juspay/hyperswitch/discussions), where you can ask for support, discuss roadmap, and share ideas.

Join our Conversation in [Slack](https://join.slack.com/t/hyperswitch-io/shared_invite/zt-2jqxmpsbm-WXUENx022HjNEy~Ark7Orw), [Discord](https://discord.gg/wJZ7DVW8mm), [Twitter](https://x.com/hyperswitchio)

When you want others to use the changes you have added you need to package it and then index it
```bash
# To package and index the new changes
task pihh
# To update readme file
task ur
```

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| file://../hyperswitch-app | hyperswitch-app | 0.2.4 |
| file://../hyperswitch-web | hyperswitch-web | 0.2.4 |
| https://prometheus-community.github.io/helm-charts | prometheus | 27.8.0 |

## Values
<h3>Other Values</h3>
<table>

<thead>
	<th >Key</th>
	<th >Default</th>
	<th >Description</th>
</thead>

<tbody><tr>
    <td><div><a href="./values.yaml#L19">global.tolerations</a></div></td>
    <td><div><code>[]</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L184">prometheus.enabled</a></div></td>
    <td><div><code>true</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L187">prometheus.global.evaluation_interval</a></div></td>
    <td><div><code>"15s"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L186">prometheus.global.scrape_interval</a></div></td>
    <td><div><code>"15s"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L182">prometheus.host</a></div></td>
    <td><div><code>"prometheus-server"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L183">prometheus.port</a></div></td>
    <td><div><code>80</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L189">prometheus.prometheus-pushgateway.tolerations</a></div></td>
    <td><div><code>[]</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L193">prometheus.scrape_configs[0].job_name</a></div></td>
    <td><div><code>"kubernetes-pods"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L195">prometheus.scrape_configs[0].kubernetes_sd_configs[0].role</a></div></td>
    <td><div><code>"pod"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L198">prometheus.scrape_configs[0].relabel_configs[0].action</a></div></td>
    <td><div><code>"keep"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L199">prometheus.scrape_configs[0].relabel_configs[0].regex</a></div></td>
    <td><div><code>true</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L197">prometheus.scrape_configs[0].relabel_configs[0].source_labels[0]</a></div></td>
    <td><div><code>"__meta_kubernetes_pod_annotation_prometheus_io_scrape"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L201">prometheus.scrape_configs[0].relabel_configs[1].action</a></div></td>
    <td><div><code>"replace"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L203">prometheus.scrape_configs[0].relabel_configs[1].regex</a></div></td>
    <td><div><code>"(.+)"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L200">prometheus.scrape_configs[0].relabel_configs[1].source_labels[0]</a></div></td>
    <td><div><code>"__meta_kubernetes_pod_annotation_prometheus_io_path"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L202">prometheus.scrape_configs[0].relabel_configs[1].target_label</a></div></td>
    <td><div><code>"__metrics_path__"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L205">prometheus.scrape_configs[0].relabel_configs[2].action</a></div></td>
    <td><div><code>"replace"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L207">prometheus.scrape_configs[0].relabel_configs[2].regex</a></div></td>
    <td><div><code>"([^:]+)(?::\\d+)?;(\\d+)"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L208">prometheus.scrape_configs[0].relabel_configs[2].replacement</a></div></td>
    <td><div><code>"$1:$2"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L204">prometheus.scrape_configs[0].relabel_configs[2].source_labels[0]</a></div></td>
    <td><div><code>"__address__"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L204">prometheus.scrape_configs[0].relabel_configs[2].source_labels[1]</a></div></td>
    <td><div><code>"__meta_kubernetes_pod_annotation_prometheus_io_port"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L206">prometheus.scrape_configs[0].relabel_configs[2].target_label</a></div></td>
    <td><div><code>"__address__"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L210">prometheus.scrape_configs[1].job_name</a></div></td>
    <td><div><code>"kubernetes-nodes"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L212">prometheus.scrape_configs[1].kubernetes_sd_configs[0].role</a></div></td>
    <td><div><code>"node"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L214">prometheus.scrape_configs[1].relabel_configs[0].action</a></div></td>
    <td><div><code>"labelmap"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L215">prometheus.scrape_configs[1].relabel_configs[0].regex</a></div></td>
    <td><div><code>"__meta_kubernetes_node_label_(.+)"</code></div></td>
    <td></td>
  </tr><tr>
    <td><div><a href="./values.yaml#L218">prometheus.scrape_configs[1].static_configs[0].targets[0]</a></div></td>
    <td><div><code>"node-exporter:9100"</code></div></td>
    <td></td>
  </tr>
</tbody>
</table>

