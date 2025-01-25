# WinForms

## Getting Started

### Create new project

> [!NOTE]
> To create a new `WinForms` project, you can use the Framas project templates available in `Framas.Templates`.
> Make sure you have already installed the `Framas.Templates`. If not, please see the instructions in [Getting Started > Install Framas.Templates](/README?id=install-framastemplates).

**Follow the steps below to create a new WinForms project**

![](/assets/gif/new_winform_project.gif)

**Project Parameters**

- AppId: The application identifier must be unique.
- AppName: The name of the application.
- MESOCOMP: The company (tenant) identifier of the application.
- SystemConnectionString: The system connection string of the application (System Db will store all configurations, libraries, plugins...) that serve the application.

**Results**

![](/assets/img/new_winform_project_result.png)

- FramasWinformApp is the main app project
- FramasWinformApp.Plugin.Main is the main plugin of the FramasWinformApp

### Create new plugin

> [!NOTE]
> To create a new `WinForms` project, you can use the Framas project templates available in `Framas.Templates`.
> Make sure you have already installed the `Framas.Templates`. If not, please see the instructions in [Getting Started > Install Framas.Templates](/README?id=install-framastemplates).

**Follow the steps below to create a new WinForms plugin**

![](/assets/gif/new_winform_plugin.gif)

### How to Debug a plugin?

1. In the main app project:
- **Add Project Reference** to plugin project 

![](/assets/img/add_ref_wf_plugin.png)

- Open **FramasProgram.cs** in main app project
- In ConfigurePluginContainer use AddAssemblyFromType to the PluginModuleEntry

```cs
using Framas.BootLoader.Abstractions;
using Framas.Bootstraper;
using Microsoft.Extensions.Configuration;
using System.Windows.Forms;

namespace FramasWinformApp
{
    public class FramasProgram : IFramasProgram
    {
        public void Run(string[] args)
        {
            FramasApplicationBuilder builder = FramasApplicationBuilder.Create();
            var success = builder.AddFramasWinformsApp(builder =>
            {
                builder.ConfigurePluginContainer(container =>
                {
#if DEBUG
                    // Add this line if you want to Debug plugin project FramasWinformPlugin
                    container.AddAssemblyFromType<FramasWinformPlugin.PluginModuleEntry>();
#endif
                });
            });

            // If the application was not created, then exit the application
            if (success)
            {
                var app = builder.Build();
                app.Run();
            }
            else
            {
                Application.Exit();
            }
        }
    }
}
```

## F Class

The `F` class is a utility helper class that provides some useful functions in WinForms applications.

### Properties

```cs
/// <summary>
/// The current logged in user
/// </summary>
public static ICurrentUser CurrentUser => GetRequiredService<ICurrentUser>();

/// <summary>
/// The current company (tenant) that application is running on
/// </summary>
public static ICurrentTenant CurrentTenant => GetRequiredService<ICurrentTenant>();

/// <summary>
/// The current application information
/// </summary>
public static ICurrentApp CurrentApp => GetRequiredService<ICurrentApp>();

/// <summary>
/// The current application directory
/// </summary>
public static ICurrentAppDirectory CurrentAppDirectory => GetRequiredService<ICurrentAppDirectory>();

/// <summary>
/// The service that help to publish and subscribe to events in network
/// </summary>
public static IDistributedEventBus DistributedEventBus => GetRequiredService<IDistributedEventBus>();

/// <summary>
/// The service that help to publish and subscribe to events in local application 
/// </summary>
public static ILocalEventBus LocalEventBus => GetRequiredService<ILocalEventBus>();

/// <summary>
/// The service that help to send email
/// </summary>
public static IEmailSender EmailSender => GetRequiredService<IEmailSender>();

/// <summary>
/// The service that help to manage configuration
/// </summary>
public static IConfigManager ConfigManager => GetRequiredService<IConfigManager>();

/// <summary>
/// The service that help to manage BLOBs
/// </summary>
public static IBlobContainerFactory BlobContainerFactory => GetRequiredService<IBlobContainerFactory>();

/// <summary>
/// The service that help to manage db connection
/// </summary>
public static IDbConnectionContainer DbConnectionContainer => GetRequiredService<IDbConnectionContainer>();
```

### Dependency Injection
The `F` class provides some functions to get the services that were registered to the application.

```cs
/// <summary>
/// Gets a required service of type T from the service provider.
/// </summary>
/// <typeparam name="T">The type of service to retrieve.</typeparam>
/// <returns>The requested service instance.</returns>
/// <exception cref="InvalidOperationException">Thrown if the service is not found.</exception>
public static T GetRequiredService<T>()
    where T : notnull
{
    return FramasWinformsApp.Instance.Services.GetRequiredService<T>();
}

/// <summary>
/// Gets a required service of the specified type from the service provider.
/// </summary>
/// <param name="serviceType">The type of service to retrieve.</param>
/// <returns>The requested service instance.</returns>
/// <exception cref="InvalidOperationException">Thrown if the service is not found.</exception>
public static object GetRequiredService(Type serviceType)
{
    return FramasWinformsApp.Instance.Services.GetRequiredService(serviceType);
}

/// <summary>
/// Gets a service of type T from the service provider.
/// </summary>
/// <typeparam name="T">The type of service to retrieve.</typeparam>
/// <returns>The requested service instance, or null if the service is not found.</returns>
public static T? GetService<T>()
{
    return FramasWinformsApp.Instance.Services.GetService<T>();
}

/// <summary>
/// Gets a service of the specified type from the service provider.
/// </summary>
/// <param name="serviceType">The type of service to retrieve.</param>
/// <returns>The requested service instance, or null if the service is not found.</returns>
public static object? GetService(Type serviceType)
{
    return FramasWinformsApp.Instance.Services.GetService(serviceType);
}

/// <summary>
/// Gets all services of type T from the service provider.
/// </summary>
/// <typeparam name="T">The type of services to retrieve.</typeparam>
/// <returns>An enumerable of the requested service instances.</returns>
public static IEnumerable<T> GetServices<T>()
{
    return FramasWinformsApp.Instance.Services.GetServices<T>();
}

/// <summary>
/// Gets all services of the specified type from the service provider.
/// </summary>
/// <param name="serviceType">The type of services to retrieve.</param>
/// <returns>An enumerable of the requested service instances.</returns>
public static IEnumerable<object?> GetServices(Type serviceType)
{
    return FramasWinformsApp.Instance.Services.GetServices(serviceType);
}
```

### Localization
The `F` class expose property `Lang` that help you eaisly perform string localization

```cs
// Localize text
string localizedText = F.Lang["Please Wait"];

// Localize text with arguments
string localizedTextWithArgs = F.Lang["Hello {0}", "World"];
```

To change current language you can use
```cs
 var cultureInfo = new CultureInfo("en-US");
 F.Lang.ChangeLanguage(cultureInfo);
```

### Logging
The `F` class expose property `Logger` that help you logging a message

The log messages will store in table table `ST023_Log`

```cs
F.Logger.Debug("This is a debug message");
F.Logger.Information("This is an information message");
F.Logger.Warning("This is a warning message");
F.Logger.Error("This is an error message");
F.Logger.Fatal("This is a fatal message");

// you can log the exception like this
F.Logger.Error(ex, "The exception occurs");
```

The default loggin level is `Warning`

But you can also change minimum logging level by using:

```cs
F.ChangeMinimumLoggingLevel(LogEventLevel.Information);
```

### DbConnection

The `F` class provides some functions to get the DbConnection by using the `Name` of the connection that is stored in the table FT017_DbConnectionInfo.

```cs
DbConnection conn = F.CreateDbConnection("DOGE_WH");

// or you can get the connection info only
IDbConnectionInfo connInfo = F.GetDbConnectionInfo("DOGE_WH");
```

### ReportLayout

The `F` class can get the `XtraReport` that is stored in the table `FT032_ReportLayout`

```cs
// To create XtraReport by using it name
XtraReport report = F.CreateXtraReport("report1");

// To get all FT032_ReportLayout models
List<FT032_ReportLayout> reportLayout =  F.GetAllReportLayouts();

// To find specified FT032_ReportLayout by name
FT032_ReportLayout reportLayout = FindReportLayoutByName(report1);

// To convert FT032_ReportLayout to XtraReport
XtraReport report = reportLayout.ToXtraReport();
```

### Text Templating
`F` class provides a simple, yet efficient text template system. Text templating is used to dynamically render contents based on a template and a model (a data object):

The template content is stored in the table `ST034_TextTemplate`

To render the template to string
```cs
// Assume the template1 has content: Hello {{ message }}!
// Create the template definition 
TemplateDefinition templateDef = F.CreateTextTemplateDefinition("template1");

// render content using TemplateDefinition
string content = F.RenderTextTemplate(templateDef,
    model: new { Message = "World" });
// The content result will be: Hello World!

// or use can render by using the template name directly without TemplateDefinition
string content = F.RenderTextTemplate("template1",
    model: new { Message = "World" });
```

### Show Overlay over the control

If you want to block the UI during the execution of a task to ensure the user can't do anything until all tasks are completed, you can use:

```cs
// Show overlay in sync mode
F.RunWithOverlay(this, () =>
{
    // Do some sync works here
});

// Show overlay in sync mode but has cancel button
F.RunWithOverlayCancelable(this, c => () =>
{            
    // c is the CancellationToken
    // Do some sync works here
});

// Show overlay in async mode
await F.RunAsyncWithOverlay(this, async () =>
{
    // Do some async works here
});

// Show overlay in async mode but has cancel button
await F.RunAsyncWithOverlayCancelable(this, c => async () =>
{
    // c is the CancellationToken
    // Do some async works here
});
```

### Config Value
`F` class provide simple way to manage the config value in the application. The context will be identify by specified key

#### ConfigContext

ConfigContext define the config will be serve for specified context

We have 4 ConfigContext:

- `Global`: The config use for global context. The config with context Global will have only 1 config with specified key
- `Company`: The config use for current selected company (tenant) environment. The config with context Company will have only 1 config per Company (tenant)
- `User`: The config use for current logged in user. The config with context Company will have only 1 config per User
- `Machine`: The config use for current machine. The config with context Machine will have only 1 config per Machine


#### Define the config class
To create define config instance we will create new class with attribute `FramasConfig` like below.

Application will automatically create config for you when run the application

```cs
[FramasConfig(ConfigContext.Global, IsEncrypt = true, Key = "OMS.Plugin.Main.MySettings", Description = "My app settings")]
public class MySettings
{
    public string? Value1 { get; set; }
    public int Value2 { get; set; }
    public bool Value3 { get; set; }

    public ObjectData ObjectData { get; set; } = new ObjectData();

    public List<string> ListStrings { get; set; } = [];
    public List<int> ListNumbers { get; set; } = [];
    public List<bool> ListBools { get; set; } = [];
}

// For class value add TypeConverter to allow edit when edit the config
[TypeConverter(typeof(ExpandableObjectConverter))]
public class ObjectData
{
    public string? Value1 { get; set; }
    public string? Value2 { get; set; }
}
```

#### FramasConfigAttribute

FramasConfig have several properties below:
- `ConfigContext`: (required): The `ConfigContext` of the config
- `IsEncrypt`: (optional) (Default: false): Indicate the config data will be encrypted 
- `Key`: (optional) (Default: If not provide the key will be the class full name): The key to identify the config
- `Description`: (optional) (Default: ""): The description about the config

#### Get the config value

To get the config value we use:

```cs
// get config by GetConfig<T> generic method
MySettings mySettings = F.GetConfig<MySettings>();

// get config by type
object? configObj = F.GetConfig(typeof(MySettings));

// cast config object to target type
MySettings settings = (MySettings)configOjb!;
```

#### Edit config use edit form

To edit the config value we use ShowEditConfigForm this will show the dialog to update config

```cs
F.ShowEditConfigForm<MySettings>();

F.ShowEditConfigForm(typeof(MySettings));
```

![](/assets/img/edit_config_form.png)

#### Update config via code

```cs
// try get config
MySettings settings = F.GetConfig<MySettings>();

// change config value
settings.Value1 = "New Value";

// call update config
F.UpdateConfig(settings);
```
--- 

## Components

### FramasLocalizer

This component help you automatically localization text for control, column... by scanning children of the target control

#### Support localize target control

- Form
- Control
- GridControl (for localize columns)

#### Properties

- Target (required): The target control that component will apply to
- RibbonPageGroup (optional): Set this property if you want to create language menu the that RibbonPageGroup like picture below:

![](/assets/img/framas_localizer_menu.png)

#### Events

- CultureChanged: Event occurs when the current culture of the application has changed

#### How to use

In the Form, UserControl designer. Open Toolbox find and drag component name `FramasLocalizer` in to design surface or double click on it.

- Assign `Target` property to apply localization

![](/assets/gif/how_to_use_framas_localizer.gif)

#### Open language settings dialog

You can click to the ribbon button like a picture below to open language settings dialog:

![](/assets/img/open_language_settings_dialog.png)

To edit the language please follow below steps:

![](/assets/img/edit_language_settings.png)

--- 

###  FramasGridViewLayoutManager

`FramasGridViewLayoutManager` help you save and restore the layout of the GridView

#### Properties

- Id (required): The layout identifier  (must be unique for each grid view)
- Target (required): The target BaseView that component will apply to
- UseRegistry (optional): If set true the layout will store in the Registry on the machine 
- RibbonPageGroup (optional): Set this property if you want to add layout menu to the RibbonPageGroup like picture below:

![](/assets/png/framas_gridviewlayoutmanager_menu.png)

#### Method

- SaveState(): Save current layout state
- BestFit(): Fit the column to it content

#### Events

- IdResolve: Event to resolve the Id. In case you have multiple Target to apply.

#### How to use

In the Form, UserControl designer. Open Toolbox find and drag component name `FramasGridViewLayoutManager` in to design surface or double click on it.

- Assign `Target` property to apply
- Set `Id` for `FramasGridViewLayoutManager` component

![](/assets/gif/how_to_use_framas_gridviewlayoutmanager.gif)

### FramasAdminMenu

The `FramasAdminMenu` is the component that help you add all menu that support you manage the application.

#### Properties

- ManagementRibbonPage: The RibbonPage that Management menu will be add to

![](/assets/img/admin_menu_management.png)

- ConfigurationRibbonPage: The RibbonPage that Configuration menu will be add to

![](/assets/img/admin_menu_configuration.png)

- PageContainer: The FramasPageContainer to contains the settings page

#### How to use 

In the Form, UserControl designer. Open Toolbox find and drag component name `FramasAdminMenu` in to design surface or double click on it.

- Assign `ManagementRibbonPage` property to add Management menu
- Assign `ConfigurationRibbonPage` property to add Configuration menu
- Assign `PageContainer` property

### FramasSkinController

The `FramasSkinController` is the component that help you apply theme to your application.

#### How to use 

In the Form, UserControl designer. Open Toolbox find and drag component name `FramasSkinController` in to design surface or double click on it.

- Assign `RibbonPageGroup` property to add skin menu to that page group

![](/assets/img/framas_skin_controller_menu.png)

### FramasAppStatus

`FramasAppStatus` help you add a application status info into StatusBar

#### Properties

- RibbonStatusBar: The RibbonStatusBar that component will add items to

![](/assets/img/app_status.png)

#### How to use

In the Form, UserControl designer. Open Toolbox find and drag component name `FramasAppStatus` in to design surface or double click on it.

- Assign `RibbonStatusBar`

### FramasAccordionPermissionManager 

The `FramasAccordionPermissionManager` help you manage authorization the `AccordionControl`

#### Properties

- Target (required): The target AccordionControl to authorize
- Id (required): The id of the component (must be unique for each `AccordionControl`)

#### Methods

- ShowSettingsDialog(): Show the authorization settings dialog.
- Reload(): Reload authorize states 

#### How to use

In the Form, UserControl designer. Open Toolbox find and drag component name `FramasAccordionPermissionManager` in to design surface or double click on it.

- Assign `Target`
- Assign `Id`

![](/assets/gif/how_to_use_framas_accordionpermissionmanager.gif)

#### Open settings dialog

You can use the function `ShowSettingsDialog` to open the settings dialog

![](/assets/img/settings_accordioncontrolpermissionmanager_steps.png)

### FramasRibbonPermissionManager 

The `FramasRibbonPermissionManager` help you manage authorization the `RibbonControl`

#### Properties

- Target (required): The target RibbonControl to authorize
- Id (required): The id of the component (must be unique for each `RibbonControl`)

#### Methods

- ShowSettingsDialog(): Show the authorization settings dialog.
- Reload(): Reload authorize states 

#### How to use

In the Form, UserControl designer. Open Toolbox find and drag component name `FramasRibbonPermissionManager` in to design surface or double click on it.

- Assign `Target`
- Assign `Id`

![](/assets/gif/how_to_use_framas_ribboncontrolpermissionmanager.gif)

#### Open settings dialog

You can use the function `ShowSettingsDialog` to open the settings dialog

![](/assets/img/settings_ribboncontrolpermissionmanager_steps.png)

### FramasControlPermissionManager 

The `FramasControlPermissionManager` help you manage authorization the Control, Form and the children inside the control or form

#### Properties

- Target (required): The target control, form to authorize
- AdornerUIManager (required): The `AdornerUIManager` require for open settings overlay

#### Methods

- ShowSettingsOverlay(): Show the authorization settings overlay.
- Reload(): Reload authorize states 

#### How to use

In the Form, UserControl designer. Open Toolbox find and drag component name `FramasControlPermissionManager` in to design surface or double click on it.

- Assign `Target`
- Assign `Id`

![](/assets/gif/how_to_use_framas_controlpermissionmanager.gif)

To show the settings overlay:

```cs
private void button1_Click(object sender, EventArgs e)
{
    framasControlPermissionManager1.ShowSettingsOverlay();
}
```

![](/assets/img/show_control_permissionsettings_overlay.png)

![](/assets/img/control_permission_settings_form.png)




