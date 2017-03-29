# DeveloperSpecificConfig
Amends the Visual Studio build process allowing each developer to work with their own settings in the web.config or app.config files.
Currently requires Visual Studio 2015 (v14) to be installed.

## How to Use
1. Copy the UserSpecificConfig.targets file to the root of your solution.
2. Edit the desired .csproj file
3. Add the following just inside the closing </Project> tag:
   Note: you may need to implement this slightly differently if you already have an active AfterBuild target in your csproj file.
   
        <PropertyGroup>
          <AfterBuildDependsOn/>
        </PropertyGroup>
        <PropertyGroup>
          <SolutionDir Condition=" '$(SolutionDir)' == '' Or '$(SolutionDir)' == '*Undefined*'">..\</SolutionDir>
        </PropertyGroup>
        <Import Project="$(SolutionDir)UserSpecificConfig.targets"/>
        <Target Name="AfterBuild" DependsOnTargets="$(AfterBuildDependsOn)">
        </Target>

