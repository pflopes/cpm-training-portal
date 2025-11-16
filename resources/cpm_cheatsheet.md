# Central Package Management Cheat Sheet

## Basic Setup

### 1. Create Directory.Packages.props

```xml
<Project>
  <PropertyGroup>
    <ManagePackageVersionsCentrally>true</ManagePackageVersionsCentrally>
  </PropertyGroup>
  <ItemGroup>
    <PackageVersion Include="Newtonsoft.Json" Version="13.0.3" />
    <PackageVersion Include="Microsoft.Extensions.Logging" Version="8.0.0" />
  </ItemGroup>
</Project>
```

### 2. Update Project Files

```xml
<ItemGroup>
  <PackageReference Include="Newtonsoft.Json" />
  <PackageReference Include="Microsoft.Extensions.Logging" />
</ItemGroup>
```

## Advanced Features

### Version Override

```xml
<PackageReference Include="Newtonsoft.Json" VersionOverride="12.0.3" />
```

### Transitive Pinning

```xml
<PropertyGroup>
  <CentralPackageTransitivePinningEnabled>true</CentralPackageTransitivePinningEnabled>
</PropertyGroup>
```

### Global Package References

```xml
<ItemGroup>
  <GlobalPackageReference Include="SonarAnalyzer.CSharp" Version="9.19.0.84025" />
</ItemGroup>
```

### Framework-Specific Versions

```xml
<PackageVersion Include="PackageA" Version="1.0.0" 
               Condition="'$(TargetFramework)' == 'netstandard2.0'" />
<PackageVersion Include="PackageA" Version="2.0.0" 
               Condition="'$(TargetFramework)' == 'net8.0'" />
```

### Disable CPM for a Project

```xml
<PropertyGroup>
  <ManagePackageVersionsCentrally>false</ManagePackageVersionsCentrally>
</PropertyGroup>
```

### Disable Version Override Feature

```xml
<PropertyGroup>
  <CentralPackageVersionOverrideEnabled>false</CentralPackageVersionOverrideEnabled>
</PropertyGroup>
```

## CLI Commands

### Create props file template (if available)

```bash
dotnet new packagesprops
```

### Install CentralisedPackageConverter tool

```bash
dotnet tool install CentralisedPackageConverter --global
```

### Convert solution to use CPM

```bash
central-pkg-converter /PATH_TO_YOUR_SOLUTION
```

### List all package versions in a solution (PowerShell)

```powershell
$packages = Get-ChildItem -Filter *.csproj -Recurse |
    Get-Content |
    Select-String -Pattern '<PackageReference Include="([^"]+)" Version="([^"]+)"' -AllMatches |
    ForEach-Object { $_.Matches } |
    Group-Object { $_.Groups[1].Value } |
    ForEach-Object { @{
        Name = $_.Name
        Versions = $_.Group.ForEach({ $_.Groups[2].Value }) | Select-Object -Unique
    }} |
    Sort-Object { $_.Name }

$packages | ForEach-Object {
    "$($_.Name) versions:"
    $_.Versions | ForEach-Object { "  $_" }
}
```

## Common Issues & Solutions

### NU1507 Warning

**Issue**: Warning about multiple package sources with CPM

**Solution**: Use package source mapping or specify a single package source

### Missing Package Versions

**Issue**: Build errors about missing package versions

**Solution**: Ensure all packages referenced in project files have corresponding entries in Directory.Packages.props

### Version Conflicts

**Issue**: Conflicts between different required versions

**Solution**: Use version overrides or update code to be compatible with a single version

## Best Practices

- Implement CPM in a separate PR/branch
- Document version override reasons with comments
- Use global package references for analyzers and code quality tools
- Schedule regular security reviews of package versions
- Don't use version overrides without clear justification
- Don't make all packages global; only use for truly common dependencies

## Requirements

- NuGet 6.2 or newer
- .NET SDK 6.0.300 or newer
- Visual Studio 2022 17.2 or newer (if using VS)

## Resources

- [Microsoft Learn: Central Package Management](https://learn.microsoft.com/en-us/nuget/consume-packages/central-package-management)
- [.NET Blog: Introducing Central Package Management](https://devblogs.microsoft.com/dotnet/introducing-central-package-management/)
- [GitHub: NuGet Central Package Management Samples](https://github.com/NuGet/Samples/tree/main/CentralPackageManagement)
