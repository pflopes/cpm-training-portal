# Central Package Management FAQ

## General Questions

### What is Central Package Management (CPM)?

Central Package Management is a NuGet feature that allows you to manage package versions for all projects in a solution from a single, central location. It separates the declaration of package dependencies from their version specifications, making it easier to maintain consistent versions across multiple projects.

### When was CPM introduced?

CPM was introduced in NuGet 6.2 in April 2022.

### What are the requirements to use CPM?

To use Central Package Management, you need:
- NuGet 6.2 or newer
- .NET SDK 6.0.300 or newer
- Visual Studio 2022 17.2 or newer (if using Visual Studio)

### Is CPM compatible with all project types?

CPM works with all `PackageReference`-based MSBuild projects, including SDK-style projects and legacy .csproj files that use PackageReference format (as opposed to packages.config).

### Does CPM work with .NET Framework projects?

Yes, CPM works with .NET Framework projects that use the PackageReference format. It does not work with projects using the older packages.config format.

## Implementation Questions

### How do I enable CPM in my solution?

Create a file named `Directory.Packages.props` at the root of your solution with the following content:

```xml
<Project>
  <PropertyGroup>
    <ManagePackageVersionsCentrally>true</ManagePackageVersionsCentrally>
  </PropertyGroup>
  <ItemGroup>
    <PackageVersion Include="PackageName" Version="X.Y.Z" />
  </ItemGroup>
</Project>
```

Then update your project files to reference packages without specifying versions.

### Can I use the .NET CLI to create a Directory.Packages.props file?

Some versions of the .NET SDK include a template for this:

```bash
dotnet new packagesprops
```

If this command is not available in your version, you'll need to create the file manually.

### How do I reference a package in my project file when using CPM?

Simply include the package reference without a version attribute:

```xml
<PackageReference Include="PackageName" />
```

### What happens if I specify a version in both places?

If you specify a version in both the `Directory.Packages.props` file and the project file, the version in the project file will take precedence unless you're using the `VersionOverride` attribute.

### Can I disable CPM for specific projects?

Yes, you can disable CPM for a specific project by setting the `ManagePackageVersionsCentrally` property to `false` in that project's file:

```xml
<PropertyGroup>
  <ManagePackageVersionsCentrally>false</ManagePackageVersionsCentrally>
</PropertyGroup>
```

## Advanced Features

### How do I override a package version for a specific project?

Use the `VersionOverride` attribute in the project file:

```xml
<PackageReference Include="PackageName" VersionOverride="X.Y.Z" />
```

### Can I disable the version override feature?

Yes, you can disable version overrides by setting the `CentralPackageVersionOverrideEnabled` property to `false`:

```xml
<PropertyGroup>
  <CentralPackageVersionOverrideEnabled>false</CentralPackageVersionOverrideEnabled>
</PropertyGroup>
```

### What is transitive pinning and how do I enable it?

Transitive pinning allows you to control versions of transitive dependencies (dependencies of your dependencies). Enable it by setting:

```xml
<PropertyGroup>
  <CentralPackageTransitivePinningEnabled>true</CentralPackageTransitivePinningEnabled>
</PropertyGroup>
```

### How do I add a package to all projects automatically?

Use the `GlobalPackageReference` element in your `Directory.Packages.props` file:

```xml
<ItemGroup>
  <GlobalPackageReference Include="PackageName" Version="X.Y.Z" />
</ItemGroup>
```

### How do I specify different versions for different target frameworks?

Use conditions in your `PackageVersion` elements:

```xml
<PackageVersion Include="PackageName" Version="1.0.0" 
               Condition="'$(TargetFramework)' == 'netstandard2.0'" />
<PackageVersion Include="PackageName" Version="2.0.0" 
               Condition="'$(TargetFramework)' == 'net8.0'" />
```

### Can I have multiple Directory.Packages.props files?

Yes, you can have multiple `Directory.Packages.props` files in your repository. The file closest to a project's directory will be used for that project.

## Migration Questions

### How do I migrate an existing solution to use CPM?

1. Create a `Directory.Packages.props` file at the solution root
2. Extract package versions from all project files
3. Remove version attributes from `PackageReference` elements
4. Build the solution and fix any version conflicts

### Is there a tool to automate migration?

Yes, you can use the `CentralisedPackageConverter` tool:

```bash
dotnet tool install CentralisedPackageConverter --global
central-pkg-converter /PATH_TO_YOUR_SOLUTION
```

### Should I migrate all solutions at once?

It's generally better to migrate one solution at a time, starting with smaller ones to gain experience before tackling larger solutions.

### What's the best way to handle version conflicts during migration?

Document all conflicts and make deliberate decisions about which version to standardize on. Consider:
- Security implications of each version
- API compatibility requirements
- Long-term support status
- Team familiarity with different versions

## Troubleshooting

### Why am I seeing NU1507 warnings?

This warning occurs when using CPM with multiple package sources. To resolve it:
- Configure package source mapping in your `NuGet.config` file, or
- Use a single package source

### Why are some packages not being restored with the correct version?

Check that:
1. The package is correctly defined in `Directory.Packages.props`
2. The project reference doesn't include a `Version` attribute
3. There are no `VersionOverride` attributes affecting it
4. You don't have multiple `Directory.Packages.props` files with conflicting definitions

### How do I debug version resolution issues?

Use the `dotnet restore --verbosity detailed` command to see detailed information about package resolution.

### What if I need to use different versions of the same package in different projects?

Use the `VersionOverride` attribute in the projects that need a different version:

```xml
<PackageReference Include="PackageName" VersionOverride="X.Y.Z" />
```

## Best Practices

### Should I commit Directory.Packages.props to source control?

Yes, `Directory.Packages.props` should be committed to source control just like project files.

### How often should I update package versions?

Establish a regular schedule for reviewing and updating packages, with priority given to security updates. Many teams do this monthly or quarterly.

### Should I use exact versions or version ranges?

For fintech applications, it's generally better to use exact versions (e.g., `8.0.0` instead of `8.0.*`) to ensure consistency and predictability, especially for security-critical packages.

### How should I document version decisions?

Add comments in the `Directory.Packages.props` file explaining why specific versions were chosen, especially for security-critical packages or when using older versions.

### Should I use global package references extensively?

Use global package references judiciously, primarily for analyzers, code quality tools, and other packages that truly need to be included in all projects.

## Fintech-Specific Questions

### How does CPM help with security in fintech applications?

CPM helps with security by:
- Ensuring consistent security package versions across all projects
- Making it easier to update security-critical packages quickly
- Providing a single point of control for security audits
- Reducing the risk of using vulnerable package versions

### How does CPM help with compliance requirements?

CPM assists with compliance by:
- Simplifying the audit trail for package versions
- Making it easier to verify that approved versions are used
- Providing documentation of package version decisions
- Enabling consistent dependency management across teams

### Are there any fintech-specific packages that benefit particularly from CPM?

Security and compliance packages benefit significantly from centralized management:
- Authentication and authorization packages
- Encryption libraries
- Audit logging frameworks
- Regulatory compliance tools
- Financial calculation libraries

### How can I ensure my team follows CPM best practices?

- Include CPM usage in your team's coding standards
- Add CPM checks to your CI/CD pipeline
- Conduct regular training sessions
- Implement PR reviews that check for proper CPM usage
