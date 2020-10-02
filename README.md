# tye-demo
The solution consists of two projects:
- frontend and
- backend

If you want to develop and debug the demo application, please note that the configuration of the *frontend* must hold the correct URL of the backend:

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

When running allin with Tye (in KUbernetes), this configuration entry is not required. The url is in that case looked up by the Tye runtime.

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

