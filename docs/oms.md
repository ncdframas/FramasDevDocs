# WinForms

### OMS application elements

![](/assets/img/oms_app_elements.png)

**Currently OMS have 7 main module** 

- `Orders`: 
- `Materials`:
- `Molds`:
- `Machines`:
- `Manpower`:
- `Tools`:
- `Reports`:

**Each main module will have different side navigation menu**

### Create new OMS plugin project

> [!NOTE]
> Before you started please make sure you have already installed the `Framas.Templates`. If not, please see the instructions in [Getting Started > Install Framas.Templates](/README?id=install-framastemplates).

**To create new OMS plugin. First of all you need to create new branch to store the code so you can follow below steps to create new branch to develop new plugin.**

> The OMS repository url is https://github.com/framasVNIT/OMS_App
>
> The repo is private so if you need access to the repo please contact tuan.le@framas.com

> [!WARNING]
> For easily managing plugins, the branch name and the project plugin name should follow the format `OMS.Plugin.{ModuleName}.{PluginName}`. This branch is the branch main for the plugin
>
> Also add one more branch for develop like this `OMS.Plugin.{ModuleName}.{PluginName}_Dev`. We will develop on this branch


> For example: `OMS.Plugin.Orders.OpenOrders` - This is main branch for plugin
>
> For example: `OMS.Plugin.Orders.OpenOrders_Dev` - This is develop branch for plugin

![](/assets/gif/new_plugin_repo.gif)


**The plugin project structure**

![](/assets/img/oms_project_structure.png)

### How to debug the plugin before publish

![](/assets/img/how_to_debug_oms.png)


### How to contribute the side navigation menu

Open the `SideMenu.cs` and you will see property `Module` that is inherited from interface `ISideMenuContributor`. This property value will indicate the `SourceAccordionControl` will contribute for which `Module` so you can change it to contibute the menu items to another `Module`

![](/assets/img/side_menu.png)

Open designer of `SideMenu.cs` and start to contribute menu elements

### Switch beetween fVN, fFT, fIN, fKV, fGK, environments

![](/assets/img/switch_oms_env.png)

### Add OMS.Plugin.SharedModels to project

The `OMS.Plugin.SharedModels` is the project that contains all the models, classes... that shared between plugins.

To add `OMS.Plugin.SharedModels` project. Please follow below steps

1. Clone repo https://github.com/framasVNIT/OMS_App_SharedModels

> [!WARNING]
> The `OMS.Plugin.SharedModels` repo should clone into the same parent folder of the OMS folder
> 
> ![](/assets/img/clone_oms_shared_models.png)

2. In the OMS solution -> Right Click to solution -> Add Existing project

![](/assets/img/add_existing_project.png)


3. Find the project `OMS.Plugin.SharedModels.csproj` that we just clone.
