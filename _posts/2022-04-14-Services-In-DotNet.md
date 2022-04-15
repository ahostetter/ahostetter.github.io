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

`Program.cs`

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

`Worker.cs`

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



[Worker-Services-in-.NET-Core]: https://www.youtube.com/watch?v=PzrTiz_NRKA
