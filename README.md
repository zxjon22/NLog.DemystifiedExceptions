# NLog.DemystifiedExceptions [![NuGet Pre Release](https://buildstats.info/nuget/NLog.DemystifiedExceptions?includePreReleases=false)](https://www.nuget.org/packages/NLog.DemystifiedExceptions)
Improve your productivity - log clean, easy to understand exception stack traces using @benaadams's [Demystifier](https://github.com/benaadams/Ben.Demystifier).

# Installation

Install from NuGet:

```powershell
Install-Package NLog.DemystifiedExceptions
```

You should not need to make any changes to the `NLog` configuration file for your application. The package will override `NLog`'s built-in `${exception}` layout renderer automatically.

# Troubleshooting

### Configuration

`NLog` should automatically detect and load the `NLog.DemystifiedExceptions.dll` assembly as required.

If you have a scenario where this does not happen, you may have to register the package manually:
```csharp
using NLog.LayoutRenderers;

LayoutRenderer.Register<DemystifiedExceptionLayoutRenderer>("exception");
```

### Duplicate Exceptions Logged

Using the following code to log an exception will cause the exception to be logged twice:

```csharp
logger.Error(ex);
```

This happens because `NLog` will use the original exception stack trace for `${message}` and the demystified version for `${exception}` when you have this kind of configuration:

```xml
  <targets>
    <target xsi:type="ColoredConsole" name="console" layout="${longdate} ${uppercase:${level}} ${message} ${exception:format=tostring} "/>
  </targets>
```

To avoid the duplicates, always pass a message string, even if it is empty:

```charp
logger.Error(ex, string.Empty);
```

# Credits

Implementation based on discussions in the NLog issue tracker:
https://github.com/NLog/NLog/issues/2159

With thanks to everybody involved!
