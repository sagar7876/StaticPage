## File: `SerialogWithSeq/Controllers/WeatherForecastController.cs`

```csharp
using Microsoft.AspNetCore.Mvc;
using Serilog;

namespace SerialogWithSeq.Controllers
{
    [ApiController]
    [Route("[controller]")]
    public class WeatherForecastController : ControllerBase
    {
        private static readonly string[] Summaries = new[]
        {
            "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
        };

        private readonly ILogger<WeatherForecastController> _logger;

        public WeatherForecastController(ILogger<WeatherForecastController> logger)
        {
            _logger = logger;
        }

        [HttpGet(Name = "GetWeatherForecast")]
        public IEnumerable<WeatherForecast> Get()
        {
            Log.Information("Weather get info start");
            try
            {
                int zero = 0;
                int b = 5 / zero;
                return Enumerable.Range(1, 5).Select(index => new WeatherForecast
                {
                    Date = DateOnly.FromDateTime(DateTime.Now.AddDays(index)),
                    TemperatureC = Random.Shared.Next(-20, 55),
                    Summary = Summaries[Random.Shared.Next(Summaries.Length)]
                })
                .ToArray();
            }
            catch (Exception ex)
            {
                Log.Error("Weather get info error : ", ex.Message);
                throw;
            }
        }
    }
}

```

## File: `SerialogWithSeq/Program.cs`

```csharp
using Serilog;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.

builder.Services.AddControllers();
// Learn more about configuring OpenAPI at https://aka.ms/aspnet/openapi
builder.Services.AddOpenApi();

var app = builder.Build();

//Log.Logger = new LoggerConfiguration()
//    .MinimumLevel.Information()  // Adjust the minimum level as needed
//    .WriteTo.Seq("http://localhost:5341") // The URL where your Seq instance is running
//    .CreateLogger();

Log.Logger = new LoggerConfiguration()
    .MinimumLevel.Information()
    .Enrich.WithMachineName()     // Logs the machine name
    .Enrich.WithProcessId()       // Logs the process ID
    .Enrich.WithThreadId()        // Logs the thread ID
    .WriteTo.Seq("http://localhost:5341") // Your Seq server URL
    .CreateLogger();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();
}

app.UseAuthorization();

app.MapControllers();

app.Run();

```

## File: `SerialogWithSeq/WeatherForecast.cs`

```csharp
namespace SerialogWithSeq
{
    public class WeatherForecast
    {
        public DateOnly Date { get; set; }

        public int TemperatureC { get; set; }

        public int TemperatureF => 32 + (int)(TemperatureC / 0.5556);

        public string? Summary { get; set; }
    }
}

```

