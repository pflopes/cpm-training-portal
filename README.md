# Central Package Management in .NET - Training Portal

A comprehensive training portal for learning Central Package Management (CPM) in .NET, specifically tailored for fintech developers.

## Overview

This training portal provides everything you need to master Central Package Management in .NET:

- **Interactive Slides**: 10 comprehensive slides covering all aspects of CPM
- **Downloadable Resources**: Guides, cheat sheets, lab exercises, and FAQs
- **Practical Demo Project**: A complete FintechDemo solution showcasing CPM implementation
- **Code Examples**: Real-world examples with syntax highlighting

## Features

### Training Slides
1. **Introduction** - Overview of the training session
2. **The Problem** - Understanding challenges in multi-project solutions
3. **What is CPM?** - Core concepts and benefits
4. **Enabling CPM** - Step-by-step implementation guide
5. **Advanced Features** - Version overrides, transitive pinning, and more
6. **Fintech Benefits** - Specific advantages for fintech applications
7. **Migration** - Strategies for existing projects
8. **Demo Project** - Introduction to the practical example
9. **Best Practices** - Recommended approaches for success
10. **Conclusion** - Summary and additional resources

### Downloadable Resources
- **Comprehensive Guide** - Detailed documentation covering all aspects of CPM
- **Quick Reference Cheat Sheet** - Quick lookup for common commands and patterns
- **Hands-on Lab Exercise** - Step-by-step instructions for implementing CPM
- **FAQ Document** - Answers to common questions and troubleshooting

### Demo Project
A complete .NET solution demonstrating CPM in a fintech context:
- **FintechDemo.Core** - Domain models and interfaces
- **FintechDemo.Infrastructure** - Data access layer
- **FintechDemo.Api** - REST API endpoints
- **FintechDemo.Tests** - Unit tests with xUnit and Moq

## Getting Started

1. Open the training portal in your browser
2. Navigate through the slides using the interactive viewer
3. Download resources for offline reference
4. Review the demo project structure and code examples
5. Complete the hands-on lab exercise

## Key Concepts

### Central Package Management
- Single source of truth for package versions
- Consistent dependencies across all projects
- Simplified security and compliance management
- Flexible version overrides when needed

### Benefits for Fintech
- **Enhanced Security** - Consistent security package versions
- **Compliance** - Easier auditing and verification
- **Operational Efficiency** - Reduced maintenance overhead
- **Risk Reduction** - Lower chance of version conflicts

## Requirements

To use Central Package Management, you need:
- NuGet 6.2 or newer
- .NET SDK 6.0.300 or newer
- Visual Studio 2022 17.2 or newer (optional)

## Directory Structure

```
cpm-website/
├── index.html              # Main portal page
├── slides/                 # Interactive slide presentations
│   ├── intro.html
│   ├── problem.html
│   ├── what_is_cpm.html
│   ├── enabling_cpm.html
│   ├── advanced_features.html
│   ├── fintech_benefits.html
│   ├── migration.html
│   ├── demo_intro.html
│   ├── best_practices.html
│   └── conclusion.html
├── resources/              # Downloadable documentation
│   ├── cpm_guide.md
│   ├── cpm_cheatsheet.md
│   ├── cpm_lab_exercise.md
│   └── cpm_faq.md
└── README.md              # This file
```

## Quick Reference

### Create Directory.Packages.props

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

### Update Project Files

```xml
<ItemGroup>
  <PackageVersion Include="PackageName" />
</ItemGroup>
```

## Resources

- [Microsoft Learn: Central Package Management](https://learn.microsoft.com/en-us/nuget/consume-packages/central-package-management)
- [.NET Blog: Introducing Central Package Management](https://devblogs.microsoft.com/dotnet/introducing-central-package-management/)
- [GitHub: NuGet Central Package Management Samples](https://github.com/NuGet/Samples/tree/main/CentralPackageManagement)

## License

This training material is provided as-is for educational purposes.

## Support

For questions or issues related to this training portal, please refer to the FAQ document or the comprehensive guide included in the resources section.

---

**Created for fintech development teams to master .NET dependency management.**
