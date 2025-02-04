# Getting Started

## Setup Enviroment

### Requirements

- `Visual Studio 2022` or higher
- `NET 8` or higher
- `Devexpress`

### Install framascli

```terminal
dotnet tool install --global framascli
```

> **If you already installed then run update cmd to get latest version**

```terminal
dotnet tool update framascli
```

You can visit https://www.nuget.org/packages/framascli to check it out.

### Configure framascli

Run below command to configure SystemConnectionString for framascli

**NOTE** The `SystemConnectionString` entered below is for testing purpose. `DO NOT COPY IT`

```terminal
framascli
```

![](/assets/gif/framascli_first_configure.gif)


### Manage upload target

**UploadTarget**: is the connection which is point to the database that store system information for all framas application

Run below command to `Show, Create, Update, Delete` the upload target

```terminal
framascli manage_upload_target
```

### Install FramasVSIX

`FramasVSIX` is the Visual Studio Extensions that provide some utility tools to work with Framas application project.

> If you already installed FramasVSIX follow below steps to update to latest version

![](/assets/img/update_framasvsix.png)

You can go to this link https://marketplace.visualstudio.com/items?itemName=FramasVSIX.FramasVSIX to download and install it.

### Configure FramasVSIX

After install `FramasVSIX`. Open the `Visual Studio` and follow below steps:

**NOTE:** You should enter the same `SystemConnectionString` with the `Configure framascli` step.

![](/assets/gif/framasvsix_settings.gif)

### Install Framas.Templates

`Framas.Templates` is the package that provide some common project template for Framas application.

```terminal
dotnet new install Framas.Templates
```

The templates should now available in

```terminal
dotnet new list
```

---
|Template Name                        |Short Name                 |Language  |Tags
|-----------------------------------  |-------------------------  |--------  |-----------------------------------------
|Framas Class Plugin                  |framas_class_plugin        |[C#]      |Class Plugin Framas
|Framas Winforms Plugin               |framas_winforms_plugin     |[C#]      |Class Plugin Winforms Framas
|Framas Winforms App                  |framas_winforms_app        |[C#]      |Framas Winforms Application
|Framas AspNetCore App                |framas_aspnetcore_app      |[C#]      |Framas AspNetCore Application
|Framas AspNetCore Plugin             |framas_aspnetcore_plugin   |[C#]      |Framas AspNetCore Plugin
|OMS Plugin                           |oms_plugin                 |[C#]      |Framas AspNetCore Plugin OMS
---

Now you can open `Visual Studio` and see new templates was added

![](/assets/img/vs_project_templates.png)

### Add Framas Nuget Package Source

`Framas Nuget Package Source` is the self hosted nuget server for internal use. This server contains all packages was built by Framas dev.

You can add `Framas Nuget Package Source` into `Visual Studio` by following below steps:

1. Open Visual Studio
2. Go to Tools -> NuGet Package Manager -> Package Manager Settings -> Package Sources
3. Click `+` button -> Enter `Name` and `Source` -> OK

> You can get `Source` after host `FramasNugetServer`. The `Source` will be like `http://10.40.10.4:5860/v3/index.json`
> 
> You can contact congdat.nguyen@framas.com to get `FramasNugetServer`

![](/assets/gif/vs_add_nugetpackagesource.gif)

