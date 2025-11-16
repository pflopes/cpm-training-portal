# Central Package Management Lab Exercise

## Overview

In this hands-on lab, you will learn how to implement Central Package Management (CPM) in a .NET solution. You will start with a multi-project solution that uses traditional package management and migrate it to use CPM. This lab is designed for developers working in a fintech context, but the principles apply to any .NET solution.

## Prerequisites

- .NET SDK 8.0 or later
- Visual Studio 2022 (17.2 or later) or Visual Studio Code
- Basic understanding of NuGet package management
- Git for version control

## Lab Duration

Approximately 60 minutes

## Lab Steps

### Step 1: Clone the Demo Repository

1. Open a terminal or command prompt
2. Clone the demo repository:
   ```bash
   git clone https://github.com/yourusername/FintechDemo.git
   cd FintechDemo
   ```
3. Examine the solution structure:
   ```
   FintechDemo.sln
   ├── FintechDemo.Core/
   ├── FintechDemo.Infrastructure/
   ├── FintechDemo.Api/
   └── FintechDemo.Tests/
   ```

### Step 2: Analyze Current Package References

1. Open the solution in Visual Studio or your preferred IDE
2. Examine the package references in each project file (.csproj)
3. Note the packages used and their versions
4. Identify any inconsistencies in package versions across projects

### Step 3: Create Directory.Packages.props

1. Create a new file named `Directory.Packages.props` at the solution root
2. Add the basic structure to enable Central Package Management:
   ```xml
   <Project>
     <PropertyGroup>
       <ManagePackageVersionsCentrally>true</ManagePackageVersionsCentrally>
     </PropertyGroup>
     <ItemGroup>
       <!-- Package versions will be added here -->
     </ItemGroup>
   </Project>
   ```

### Step 4: Extract Package Versions

1. For each package reference found in Step 2, add a `PackageVersion` entry to `Directory.Packages.props`
2. Use the following format:
   ```xml
   <PackageVersion Include="PackageName" Version="X.Y.Z" />
   ```
3. For packages with different versions across projects, decide on a consistent version to use

### Step 5: Update Project Files

1. Open each project file (.csproj)
2. Remove the `Version` attribute from each `PackageReference` element
3. Save the changes

### Step 6: Build and Test

1. Build the solution:
   ```bash
   dotnet build
   ```
2. Fix any build errors related to package version changes
3. Run the tests to ensure everything works correctly:
   ```bash
   dotnet test
   ```

### Step 7: Implement Advanced Features

#### 7.1: Add a Global Package Reference

1. Add a global analyzer package to `Directory.Packages.props`:
   ```xml
   <ItemGroup>
     <GlobalPackageReference Include="SonarAnalyzer.CSharp" Version="9.19.0.84025" />
   </ItemGroup>
   ```
2. Build the solution to verify the analyzer is applied to all projects

#### 7.2: Override a Version for a Specific Project

1. Choose one project where you want to use a different package version
2. Open the project file and add a `VersionOverride` attribute:
   ```xml
   <PackageReference Include="PackageName" VersionOverride="X.Y.Z" />
   ```
3. Build the project to verify the override works

#### 7.3: Enable Transitive Pinning

1. Add the following property to `Directory.Packages.props`:
   ```xml
   <PropertyGroup>
     <CentralPackageTransitivePinningEnabled>true</CentralPackageTransitivePinningEnabled>
   </PropertyGroup>
   ```
2. Add a version for a transitive dependency:
   ```xml
   <PackageVersion Include="TransitiveDependency" Version="X.Y.Z" />
   ```
3. Build the solution to verify the transitive dependency is pinned

### Step 8: Add Framework-Specific Versions

1. Modify one of your projects to target multiple frameworks:
   ```xml
   <PropertyGroup>
     <TargetFrameworks>net8.0;net6.0</TargetFrameworks>
   </PropertyGroup>
   ```
2. Add framework-specific versions to `Directory.Packages.props`:
   ```xml
   <PackageVersion Include="PackageName" Version="X.Y.Z" Condition="'$(TargetFramework)' == 'net8.0'" />
   <PackageVersion Include="PackageName" Version="A.B.C" Condition="'$(TargetFramework)' == 'net6.0'" />
   ```
3. Build the project to verify the correct versions are used for each framework

### Step 9: Document Your Implementation

1. Create a README.md file explaining your CPM implementation
2. Include:
   - Why CPM was implemented
   - Any version decisions made
   - How to add new packages
   - How to update existing packages

### Step 10: Commit Your Changes

1. Commit your changes to version control:
   ```bash
   git add .
   git commit -m "Implemented Central Package Management"
   ```

## Bonus Challenges

### Challenge 1: Automate Migration

Use the `CentralisedPackageConverter` tool to automate the migration process on a copy of the original solution:

```bash
dotnet tool install CentralisedPackageConverter --global
central-pkg-converter /path/to/solution/copy
```

Compare the automated results with your manual implementation.

### Challenge 2: Create a Package Update Strategy

Create a document outlining a strategy for:
- When to update packages
- How to handle security updates
- Process for testing package updates
- Rollback procedures

### Challenge 3: Implement Multiple Directory.Packages.props Files

1. Create a subdirectory in your solution
2. Move one of the projects to this subdirectory
3. Create a separate `Directory.Packages.props` file in the subdirectory
4. Configure it to import the root `Directory.Packages.props` file and override some versions

## Lab Completion Checklist

- [ ] Created `Directory.Packages.props` file
- [ ] Extracted all package versions
- [ ] Updated all project files to remove version attributes
- [ ] Successfully built the solution
- [ ] All tests pass
- [ ] Implemented at least one advanced feature
- [ ] Documented the implementation
- [ ] Committed changes to version control

## Resources

- [Microsoft Learn: Central Package Management](https://learn.microsoft.com/en-us/nuget/consume-packages/central-package-management)
- [.NET Blog: Introducing Central Package Management](https://devblogs.microsoft.com/dotnet/introducing-central-package-management/)
- [GitHub: NuGet Central Package Management Samples](https://github.com/NuGet/Samples/tree/main/CentralPackageManagement)
