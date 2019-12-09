# Worker-Service-Sample 
Background Worker Service for Linux and Windows | dotnet core +3.0

------------

### How To Create .NET Core Worker Service Project
1. Use Visual Studio 2019 (VS19)
2. Open VS19 and Create a New Project
3. Search For Worker Service Template
4. Name your project and choose Location for project files
	- You can check**"Place solution and project in the same directory"**checkbox.
5. Click Create Button
6. Choose .NET SDK version (+3.0)
	- This repository using version 3.1.0
7. Click Create Button and your project is must be ready.

------------

### How To Make The Project Compatible With Linux OS

1. Open the project solution with VS19
2. In the Project tab find "Manage NuGet Packages..." option and click on it.
3. Inside the Manage NuGet Packages window switch to "Browse" tab.
4. Search for "Microsoft.Extensions.Hosting.Systemd" with version +3.0
5. Install the NuGet package
6. After installation navigate to Program.cs file on solution explorer.
7. Edit your CreateHostBuilder() method like below snippet 
```C#
        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .UseSystemd()
                .ConfigureServices((hostContext, services) =>
                {
                    services.AddHostedService<Worker>();
                });
```
8.  Save Program.cs and the project ready for work on Linux OS.
	- You can edit ExecuteAsync() in Worker.cs or override other virtual methods.
9. Publish the project using following CLI line on Linux OS
	`
	dotnet publish -o publish
	`


------------

### Service Installation Steps for Linux OS
1. Navigate to '/etc/systemd/system/'
2. Create a 'SERVICENAME.service' file and edit with text editor.
3. Use bellow template to create service file.
```
[Unit]
Description= Description of Worker Service
[Service]
Type=notify
ExecStart= Path to your published executable
[Install]
WantedBy=multi-user.target
```
4.  Save your file
5. Open terminal and reload daemon list with below line
`
sudo systemctl daemon-reload
`
6. You can use below line to manage your service
`
sudo system ctl start|stop|restart|status SERVICENAME.service
`
