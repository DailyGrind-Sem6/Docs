# Monitoring

After my research, I decided to use New Relic to monitor my Kubernetes deployment. New Relic is a monitoring tool that provides real-time insights into the performance and usage of your applications.

## Setting up New Relic

After creating an account, I clicked on `Add Data` after which I selected `Kubernetes`. This opened a modal where I selected the `Guided` installation method. This method provides a step-by-step guide on how to install the New Relic Kubernetes integration.

Eventually it will display a command which you can paste into your Kubernetes cluster to install the New Relic Kubernetes integration.

This is what the command begins with:

```bash
curl -Ls https://download.newrelic.com/install/newrelic-cli/scripts/install.sh | bash && NEW_RELIC_CLI_SKIP_CORE=1 ...
```

After running the command, the New Relic Kubernetes integration was installed on my cluster.

## Viewing the data

After the installation, I was able to view data on all my Kubernetes deployments. This data includes the CPU and memory usage of each pod, the number of requests being made to the pod, and more.

After clicking on one of the deployments, it shows data specific to that deployment. It does this in the form of graphs that are easy to read.

This is a screenshot of part of the New Relic dashboard which shows data related to replicas:

![New-Relic-Dashboard-Replicas.png](New-Relic-Dashboard-Replicas.png)

This shows all sorts of data related to the replicas of the deployment. This includes the number of replicas, replica availability, and updated replicas.

This is a screenshot of part of the New Relic dashboard which shows CPU and Memory usage of a deployment:

![New-Relic-Dashboard.png](New-Relic-Dashboard.png)
