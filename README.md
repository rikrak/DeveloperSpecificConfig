# DeveloperSpecificConfig
Amends the Visual Studio build process allowing each developer to work with their own settings in the web.config or app.config files.

## How to Use
1. Copy the `UserSpecificConfig.targets` file to the root of your solution.
2. Edit the desired .csproj file
3. Add the following just inside the closing </Project> tag:

        <Import Project="$(SolutionDir)DeveloperSpecificConfig.targets"/>

4. Or use the NuGet package: `Solid.Foundations.DeveloperSpecificConfig`

## influencing the Execution Order
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
