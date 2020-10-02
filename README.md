# tye-demo
The solution consists of two projects:
- frontend and
- backend

Front end configuration must hold the correct URL of the backend:

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


Run the tye command line in the folder with the solution.

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
