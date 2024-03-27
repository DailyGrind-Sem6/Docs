# Deploying the application to Kubernetes (Minikube)

By this part, I have already created a Docker Compose file for the application, which will spin up all necessary services for my application within Docker. In order to deploy the app to Kubernetes, I need to convert the Docker compose file to Kubernetes configuration files using `Kompose`.
I will then deploy the application to a local Kubernetes cluster using Minikube.

## Converting Docker Compose to Kubernetes

1. Navigate to the directory containing your `docker-compose.yml` file.

2. Run the `kompose convert` command. This will convert your Docker Compose file to files that you can use with `kubectl`.

```bash
kompose convert
```

You will see output similar to the following:
```bash
INFO Kubernetes file "zookeeper-service.yaml" created 
INFO Kubernetes file "kafka-service.yaml" created 
INFO Kubernetes file "frontend-service.yaml" created 
INFO Kubernetes file "api-gateway-service.yaml" created 
INFO Kubernetes file "post-service-service.yaml" created 
INFO Kubernetes file "zookeeper-deployment.yaml" created 
INFO Kubernetes file "kafka-deployment.yaml" created 
INFO Kubernetes file "frontend-deployment.yaml" created 
INFO Kubernetes file "api-gateway-deployment.yaml" created 
INFO Kubernetes file "post-service-deployment.yaml" created
```

## Deploying the Application to Kubernetes

3. Start a Kubernetes cluster with:
```Bash
minikube start
```

You will see output similar to the following:
```Bash
😄  minikube v1.32.0 on Microsoft Windows 10 Home 10.0.19045.4046 Build 19045.4046
✨  Using the docker driver based on existing profile
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
🔄  Restarting existing docker container for "minikube" ...
🐳  Preparing Kubernetes v1.28.3 on Docker 24.0.7 ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
🔎  Verifying Kubernetes components...
    ▪ Using image docker.io/kubernetesui/dashboard:v2.7.0
    ▪ Using image docker.io/kubernetesui/metrics-scraper:v1.0.8
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
💡  Some dashboard features require the metrics-server addon. To enable all features please run:

        minikube addons enable metrics-server


🌟  Enabled addons: storage-provisioner, default-storageclass, dashboard
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

4. Apply the generated Kubernetes configuration files using `kubectl apply -f ./`.

5. In order to access the frontend, you can run `minikube service frontend`. This should open a new tab in your browser with the frontend of your application.

> The code in your frontend should make a request to the API Gateway using the servicename, in my case, `api-gateway`, the Kubernetes cluster will recognize the servicename and translate it to the corresponding IP address. Although this may cause errors if the frontend is not properly configured.
>
{style="warning"}

My frontend makes use of nginX as a reverse proxy to the API Gateway to fix this issue. I changed the existing frontend Dockerfile to also include nginX and copy over the configuration file for nginX. This is the section that was added to the Dockerfile:

```Docker
FROM nginx:alpine

COPY ./.nginx/nginx.conf /etc/nginx/nginx.conf

COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 3000

CMD ["nginx", "-g", "daemon off;"]
```

> The Node section's `EXPOSE` and `CMD` commands were removed and replaced with the above code. 
> 
{style="info"}

I created a folder called `.nginx` and created a new file inside called `nginx.conf`. The `nginx.conf` file is a configuration file for nginX that sets up the reverse proxy to the API Gateway. The `nginx.conf` file looks like this:

```nNGINX
events {  }

http {
    server {
        listen 3000;
        root  /usr/share/nginx/html;
        include /etc/nginx/mime.types;
        
        location /api/ {
            proxy_pass http://api-gateway:8080;
        }
    }
}
```

This configuration sets up nginX to listen on port 3000 (my React frontend) and serve the files in `/usr/share/nginx/html`. It also sets up a reverse proxy to the API Gateway service when a request is made to `/api/`.

After making this change, I changed the API calls in my frontend to not use the host and port, but instead start from `/api/` to make the request to the API Gateway. This way, the frontend will work correctly when deployed to Kubernetes. The API calls in my frontend went from:

```JavaScript
const response = await fetch('http://localhost:8080/api/posts');
```

to:

```JavaScript
const response = await fetch('/api/posts');
```

## Kubernetes dashboard

To start the dashboard to view your Kubernetes deployment:

```Bash
minikube dashboard
```

You will see output similar to the following:

```Bash
🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
🎉  Opening http://127.0.0.1:51621/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ in your default browser...
```

## Cleaning Up
After you are finished testing out the application deployment, simply run the following command in your shell to delete the resources used.
`kubectl delete -f ./`

You can stop the minikube cluster with:
```Bash
minikube stop
```