---
layout: post
title:  "Learning about Services in .Net"
date:   2022-04-14 08:00:00 -0400
categories: jekyll update
---
This is the source for this post: [Worker Services in .NET Core][Worker-Services-in-.NET-Core]

Start by creating a new project in Visual Studio

![Alt text](https://github.com/ahostetter/ahostetter.github.io/blob/main/resources/images/project_type.png?raw=true "Project Type: Worker Service")

# Starting code for a service:

File: `Program.cs`

```c#
using HomeStatusService;

IHost host = Host.CreateDefaultBuilder(args)
    .ConfigureServices(services =>
    {
        services.AddHostedService<Worker>();
    })
    .Build();

await host.RunAsync();
```

File: `Worker.cs`

```c#
namespace HomeStatusService
{
    public class Worker : BackgroundService
    {
        private readonly ILogger<Worker> _logger;

        public Worker(ILogger<Worker> logger)
        {
            _logger = logger;
        }

        protected override async Task ExecuteAsync(CancellationToken stoppingToken)
        {
            while (!stoppingToken.IsCancellationRequested)
            {
                _logger.LogInformation("Worker running at: {time}", DateTimeOffset.Now);
                await Task.Delay(1000, stoppingToken);
            }
        }
    }
}
```

# Changes to files to add Logging to txt files using Serilog

File `Program.cs`

```C#
using HomeStatusService;
using Serilog;

Log.Logger = new LoggerConfiguration()
    .MinimumLevel.Debug()
    .MinimumLevel.Override("Microsoft", Serilog.Events.LogEventLevel.Warning)
    .Enrich.FromLogContext()
    .WriteTo.File(@"C:\temp\workerservice\LogFile.txt")
    .CreateLogger();

try
{
    Log.Information("Starting up the service");

    IHost host = Host.CreateDefaultBuilder(args)
        .ConfigureServices(services =>
        {
            services.AddHostedService<Worker>();
        })
        .UseSerilog()
        .Build();

    await host.RunAsync();

}
catch (Exception ex)
{
    Log.Fatal(ex, "There was a problem starting the service");
    return;
}
finally
{
    Log.CloseAndFlush();
}
```

File `Worker.cs`

```C#
namespace HomeStatusService
{
    public class Worker : BackgroundService
    {
        private readonly ILogger<Worker> _logger;
        private HttpClient _httpClient;

        public Worker(ILogger<Worker> logger)
        {
            _logger = logger;
        }

        //Stars the HTTP Client
        public override Task StartAsync(CancellationToken cancellationToken)
        {
            _httpClient = new HttpClient();
            return base.StartAsync(cancellationToken);
        }

        //Cleans up the HTTP Client when stopped
        public override Task StopAsync(CancellationToken cancellationToken)
        {
            _httpClient.Dispose();
            return base.StopAsync(cancellationToken);
        }

        protected override async Task ExecuteAsync(CancellationToken stoppingToken)
        {
            while (!stoppingToken.IsCancellationRequested)
            {
                //_logger.LogInformation("Worker running at: {time}", DateTimeOffset.Now);
                var result = await _httpClient.GetAsync("https://nextcloud.chadamtech.com");
                
                if (result.IsSuccessStatusCode)
                {
                    _logger.LogInformation("The website is up and running. Status Code {StatusCode}", result.StatusCode);
                }
                else
                {
                    _logger.LogError("The website is down. Status Code {StatusCode}", result.StatusCode);
                }

                await Task.Delay(5000, stoppingToken);
            }
        }
    }
}
```



[Worker-Services-in-.NET-Core]: https://www.youtube.com/watch?v=PzrTiz_NRKA
