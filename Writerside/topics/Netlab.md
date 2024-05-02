# Netlab

Netlab is an environment setup by Fontys which we can use to work on Kubernetes projects, without having to use all free credits on Azure for example.

In order to access the [Netlab environment](https://vcenter.netlab.fhict.nl/), you need to connect to the VPN using the Cisco AnyConnect Secure Mobility Client.

## Virtual Machine

Once you've accessed the Netlab environment, you can create a new virtual machine. You can choose the operating system and the amount of RAM and CPU cores you want to allocate to the VM. We were hinted towards using Linux as the operating system. After creating the VM, it will have been assigned a Dummy port for the network configuration. Switch this to another one in order to access the internet.

## K3s

Once the VM is set up, you can install K3s on the VM. K3s is a lightweight Kubernetes distribution that is easy to install and use. You can install K3s using the following command:

```bash
curl -sfL https://get.k3s.io | sh -
```

The console will prompt you for your password, and then it will start installing K3s. You will see output similar to the following:

```bash
[sudo] password for student:
[INFO] Finding release for channel stable
[INFO] Using vl.23.3+k3s1 as release
[INFO] Downloading hash https://github.com/k3s-io/k3s/releases/download/v1.23.3+k3s1/sha256sum-amd6
[INFO] Downloading binary https://github.com/k3s-io/k3s/releases/download/v1.23.3+k3s1/k3s
[INFO] Verifying binary download
[INFO] Installing k3s to /usr/local/bin/k3s
[INFO] Skipping installation of SELinux RPM
[INFO] Creating /usr/local/bin/kubectl sgmlink to k3s
```

when K3s is ready, the last line should show:

```bash
[INFO] systemd: Starting k3s
```

Check if K3s is running by using the following command:

```bash
sudo k3s kubectl get node
```

You should see output similar to the following:

```bash
NAME                 STATUS   ROLES                  AGE   VERSION
ubuntu-server-22-4   Ready    control-plane,master   10m   v1.23.3+k3s1
```

In order to run k8s commands without having to add `sudo k3s`, you can run the following command:

```bash
sudo chmod 644 /etc/rancher/k3s/k3s.yaml
```

This may have to be run again after a reboot.

## Github Actions Self-hosted Runner

In order to use Github Actions on the Netlab VM, you need to set up a self-hosted runner. This runner will execute the Github Actions workflows on the VM. You can set up a self-hosted runner using the following steps:

1. Go to your Github repository/organization and navigate to the `Settings` tab.
2. In the `Settings` tab, navigate to the Actions tab and select the `Runners` option.
3. Click on the `New self-hosted runner` button.
4. Click on `Linux` to display the commands to set up the runner on a Linux machine.

You will see output similar to the following:
```bash
Runner successfully added
Runner connection is good
# Runner settings
Enter name of work folder: [press Enter for _work]
Settings Saved.
```

Run the following to start the runner:

```bash
./run.sh
```

If everything goes well, you should see your newly created Runner in the Github Actions runner page:

![github-actions-self-hosted-runner.png](github-actions-self-hosted-runner.png)

When creating a new workflow, you can now select the self-hosted runner like this:

```yaml
jobs:
  build:
    runs-on: self-hosted
```

## Workflows

Before you can run any workflows, you need to navigate to `Organization` -> `Settings` -> `Actions` -> `Runner Groups` -> and enable `Allow public repositories` in order for the runner to be able to run workflows on public repositories.

After this, you can create a new workflow in your repository by creating a new file in the `.github/workflows` directory. this is an example file that I used for the workflow of my frontend:

```yaml
name: nelab-deploy
on:
  push:
    branches:
      - dev
  workflow_dispatch:

env:
  DEPLOYMENT_MANIFEST_PATH: ./Manifest/frontend-deployment.yaml
  SERVICE_MANIFEST_PATH: ./Manifest/frontend-service.yaml

jobs:
  deploy:
    runs-on: self-hosted
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Apply Deployment manifest
        run: sudo k3s kubectl apply -f ${{ env.DEPLOYMENT_MANIFEST_PATH }}

      - name: Apply Service manifest
        run: sudo k3s kubectl apply -f ${{ env.SERVICE_MANIFEST_PATH }}
```