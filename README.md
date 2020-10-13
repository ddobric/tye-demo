
The solution in the repository consists two projects:
- frontend and
- backend

*frontend* is an ASP.NET Core application that invokes the REST service hosted in  the *backend*.
If you want to develop and debug the demo application locally, the configuration of the *frontend* **must** hold the correct URL of the backend:

~~~
{
  "backend": "https://localhost:44316",
  "DetailedErrors": true,
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  }
}
~~~

When running all with Tye in Kubernetes, this configuration entry is **NOT** required. The url is in that case looked up by the Tye service discovery by using `Microsoft.Tye.Extensions.Configuration`.
To run the system locally, do following. Run the *tye* command line in the folder with the solution.

~~~
tye run
~~~

This will produce following output:

~~~
[12:26:11 INF] Executing application from C:\Temp\tye\frontend\frontend.csproj
[12:26:12 INF] Dashboard running on http://127.0.0.1:8000
[12:26:12 INF] Building projects
[12:26:13 INF] Launching service frontend_132e1456-f: C:\Temp\tye\frontend\bin\Debug\netcoreapp3.1\frontend.exe
[12:26:13 INF] frontend_132e1456-f running on process id 7908 bound to http://localhost:62130, https://localhost:62131
[12:26:13 INF] Replica frontend_132e1456-f is moving to a ready state
[12:26:14 INF] Selected process 7908.
[12:26:14 INF] Listening for event pipe events for frontend_132e1456-f on process id 7908
~~~

Now, you can navigate to the Tye dashboard:
~~~
http://127.0.0.1:8000
~~~

You should see this dashboard:
<img src='https://user-images.githubusercontent.com/1756871/94914236-e94cad80-04aa-11eb-9b73-4c749e17935a.png' />

Navigate to one of two frontend urls. You should see following:

<img src='https://user-images.githubusercontent.com/1756871/94919146-4436d280-04b4-11eb-98df-e60b436b814c.png' />

If you navigate to the backend url (https://localhost:63973/swagger) you should see following:

<img src='https://user-images.githubusercontent.com/1756871/94919339-a0015b80-04b4-11eb-9a0c-4aff6105ff14.png' />

### Deploy to Kubernetes

First of all you need to provision the Kubernetes cluster: https://docs.microsoft.com/en-us/azure/aks/tutorial-kubernetes-deploy-cluster.
In the last step get required credentials:

~~~
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
~~~

Additionally you also need to provide the registry where docker images of *frontend* and *backend* will be pushed (https://docs.microsoft.com/en-us/azure/aks/tutorial-kubernetes-prepare-acr).

After this is done, the tye is ready do easy deploy the solution. Navigate to the solution folder and execute following:

```
tye deploy --interactive
```

You will be prompted to enter the Container Registry (ex: 'example.azurecr.io' for Azure or 'example' for dockerhub) where the Kubernetes service is installed.

> Enter the Container Registry (ex: 'example.azurecr.io' for Azure or 'example' for dockerhub):

Here is the output of the commad:

<img src="https://user-images.githubusercontent.com/1756871/94922605-d17d2580-04ba-11eb-945f-13f5a5bc5979.png">

The deployment process has built the docker container and pushed repositories to my registry:

![image](https://user-images.githubusercontent.com/1756871/94923069-8e6f8200-04bb-11eb-8fde-9cc6899a80e4.png)

If you navigate to the Kubernetes service in the Azure Portal you will notice our two services *backend* and *frontend*:

<img src="https://user-images.githubusercontent.com/1756871/94922793-199c4800-04bb-11eb-9150-1d46f69ddfa2.png">

#### Test it out!

You should now see two pods running after deploying.

```
kubectl get pods

NAME                                             READY   STATUS    RESTARTS   AGE
backend-ccfcd756f-xk2q9                          1/1     Running   0          85m
frontend-84bbdf4f7d-6r5zp                        1/1     Running   0          85m
```

You'll have two services in addition to the built-in kubernetes service.

```
kubectl get service
NAME         TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
backend      ClusterIP   10.0.147.87   <none>        80/TCP    11s
frontend     ClusterIP   10.0.20.168   <none>        80/TCP    14s
kubernetes   ClusterIP   10.0.0.1      <none>        443/TCP   3d5h
```

You can visit the frontend application by port forwarding to the frontend service.

```
kubectl port-forward svc/frontend 5000:80
```

Now navigate to http://localhost:5000 to view the frontend application working on Kubernetes. You should see the list of weather forecasts just like when you were running locally.

💡 Currently tye does not provide a way to expose pods/services created to the public internet. We'll add features related to Ingress in future releases.

⚠️ Currently tye does not automatically enable TLS within the cluster, and so communication takes place over HTTP instead of HTTPS. This is typical way to deploy services in kubernetes - we may look to enable TLS as an option or by default in the future.

For more information see: 
Tutorial: https://github.com/dotnet/tye/blob/master/docs/tutorials/hello-tye/00_run_locally.md
Deploying: https://github.com/dotnet/tye/blob/master/docs/tutorials/hello-tye/01_deploy.md
