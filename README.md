# DeveloperSpecificConfig
Amends the Visual Studio build process allowing each developer to work with their own settings in the web.config or app.config files.

## How to Use
1. Copy the UserSpecificConfig.targets file to the root of your solution.
2. Edit the desired .csproj file
3. Add the following just inside the closing </Project> tag:

        <Import Project="$(SolutionDir)DeveloperSpecificConfig.targets"/>

4. Or use the NuGet package: Solid.Foundations.DeveloperSpecificConfig