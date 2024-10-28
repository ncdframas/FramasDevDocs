# Architecture

## Overview

Framas application architecture is built on the following principals and infrastructures:

* **Modularity**: Framas is designed to allow easily build true modular applications and systems. There are many pre-built module that i call is `Library`
and some module built by dev that i call is `Plugin`. Whith those modules we can easily plug-in and plug-out any module we want and increase the reusability of code.


* **Multi-App**: Imagine in Framas applications pool we might have many applications and each application will have and Id (AppId). In common each application will 
have independent resources. But at some point we might use some resources from another application. So with this architecture we can share resource across many application.

* **Multi-Tenancy**: Multi-Tenancy is a architecture to create SaaS applications where the hardware and software resources are shared by the companies (tenants).
Every company in Framas can be a tenant. So with this implement software can share data beetween each locations easily.
In Framas each company or data is identify by `mesocomp`. So in software we will do a same.

```mermaid
    C4Context      
    
      Enterprise_Boundary(b0, "Framas Company") {

        Container_Boundary(c_app_1, "Application Context 1") {

            Container(app1, "Application1")

            Container_Boundary(c_comp_1, "Tenants") {
                ContainerDb(fvn_db_1, "fVN Tenant DB") 
                ContainerDb(fft_db_1, "fFT Tenant DB") 
            }

            Rel(app1, fvn_db_1, "Use", "SqlConnection")
            Rel(app1, fft_db_1, "Use", "SqlConnection")

            Container_Boundary(c_module_1, "Modules") {
                Container(lib_1, "Libraries")
                Container(plugin_1, "Plugins")
            }
        }

        Container_Boundary(c_app_2, "Application Context 2") {
            Container(app2, "Application2")

            Container_Boundary(c_comp_2, "Tenants") {
                ContainerDb(fvn_db_2, "fVN Tenant DB") 
                ContainerDb(fft_db_2, "fFT Tenant DB") 
            }

            Container_Boundary(c_module_2, "Modules") {
                Container(lib_2, "Libraries")
                Container(plugin_2, "Plugins")
            }

            Rel(app2, fvn_db_2, "Use", "SqlConnection")
            Rel(app2, fft_db_2, "Use", "SqlConnection")
        }

        Container_Boundary(c_system, "System") {
            ComponentDb(system_db_1, "System DB")
        }           

        BiRel(app1, app2, "Shared")
        Rel(app1, system_db_1, "Use", "SqlConnection")
        Rel(app2, system_db_1, "Use", "SqlConnection")
        Rel(system_db_1, lib_1, "Use", "SqlConnection")
        Rel(system_db_1, plugin_1, "Use", "SqlConnection")
        Rel(system_db_1, lib_2, "Use", "SqlConnection")
        Rel(system_db_1, plugin_2, "Use", "SqlConnection")


      }
```

> One `Application` may have **many** `Tenants` </br>
> `Application` will acess to one `SystemDB` </br>
> `SystemDB` will store information about applications, tenants, plugins, libraries...

## Multi-App


## Multi-Tenancy

## Modularity

### 