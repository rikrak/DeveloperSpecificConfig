# DeveloperSpecificConfig
Amends the Visual Studio build process allowing each developer to work with their own settings in the web.config or app.config files.  It uses [config transformations](http://msdn.microsoft.com/en-us/library/dd465326.aspx) to achieve this.  Transformation files are located by looking for a file with the name `app.MACHINE_NAME.config` (`web.MACHINE_NAME.config` for asp.net projects).  If found the transforms in that file will be applied to the `app.config` (or `web.template.config`).

**NOTE:** for web projects, there needs to be a master configuration file named `web.template.config`.  This is considered the master copy of the config file.  At build time the `web.template.config` will be copied over the `web.config` and have transforms applied.


## How to Use
1. Manual
   - Copy the `UserSpecificConfig.targets` file to the root of your solution.
   - Edit the desired .csproj file
   - Add the following just inside the closing </Project> tag:

        <Import Project="$(SolutionDir)DeveloperSpecificConfig.targets"/>

2. Or use the NuGet package: `Solid.Foundations.DeveloperSpecificConfig`
3. For regular program projects:
   - Create a file named `App.MACHINE_NAME.config`  e.g. `App.Dev-Machine.config`
   - Add some transformations to the file
   - Build the project. The resulting `Program.exe.config` will be a combination of `App.config` and `App.MACHINE_NAME.config`
4. For a Web project
   - Copy the existing `web.config` into a file named `web.template.config`
   - Creat a transformation file named `web.MACHINE_NAME.config`
   - Add some transformations
   - Build the project. The resulting `web.config` will be a combination of `web.template.config` and `web.MACHINE_NAME.config`

## Influencing the Execution Order
In some cases there may be other parts of the build process that are also transforming the config file (this is particularly true of the web.config in ASP.NET projects).
In these scenarios it's important that the transfomations are executed in the correct order.  Generally the Developer specific config will need to run first as it begins from a template and creates the web.config file.

To influence the execution order there are two properties that can be set in the project file:

    <PropertyGroup>
      <RunCreateUserSpecificConfigAfter>$(RunCreateUserSpecificConfigAfter);AfterBuild</RunCreateUserSpecificConfigAfter>
      <RunCreateUserSpecificConfigBefore>$(RunCreateUserSpecificConfigBefore)</RunCreateUserSpecificConfigBefore>
    </PropertyGroup>

**Note:** these properties are optional and are only required if you want to influence the execution order of the config transformations.

For example: If there is another transformation running on the `web.config` that occurs in a target called *SetAssemblyRedirects* you would need to add the following to your project file:

    <PropertyGroup>
      <RunCreateUserSpecificConfigBefore>SetAssemblyRedirects</RunCreateUserSpecificConfigBefore>
    </PropertyGroup>
