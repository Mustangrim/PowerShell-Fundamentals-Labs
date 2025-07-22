# PowerShell Fundamentals Lab 04: PowerShell Modules

## Lab Overview

**Lab Focus:** PowerShell module management, installation, and utilization for enhanced SOC capabilities  
**Difficulty:** Intermediate    
**Author:** Mykyta Palamarchuk  
**Role:** SOC Analyst | Windows Automation & Incident Response  
**Certification:** CompTIA Security+ (SY0-701)

### Environment Specifications
- **Platform:** Windows 10/11 with PowerShell 5.1+
- **Administrative Privileges:** Required for module installation and management
- **Business Context:** Enterprise SOC environment with extended security tooling
- **Technology Focus:** Module ecosystem, package management, and security tool integration

---

## Learning Objectives

Upon completion of this lab, participants will be able to:

1. **Master module discovery** using Get-Module for inventory and version management
2. **Understand module architecture** including PSModulePath and automatic importing
3. **Install modules from PowerShell Gallery** using Install-Module for automated deployment
4. **Perform manual module installation** for custom or specialized security tools
5. **Explore module capabilities** using Get-Command for feature discovery and utilization

---

## Business Scenario

### SOC Module Ecosystem Challenge
Modern SOC operations require integration with diverse security tools and cloud platforms. PowerShell modules provide essential capabilities for AWS security analysis, Active Directory investigation, and specialized security tooling. Understanding module management enables SOC analysts to rapidly deploy new capabilities and integrate third-party security tools into their investigation workflows.

**Operational Requirements:**
- **Cloud Integration:** AWS security tools for multi-cloud environments
- **Security Tooling:** Specialized modules for advanced threat analysis
- **Version Management:** Consistent module versions across SOC workstations
- **Custom Tools:** Manual deployment of proprietary or specialized modules

---

## PowerShell Module Architecture

### Module Fundamentals

**Module Components:**
PowerShell modules are packages consisting of various PowerShell members:
- **Cmdlets:** Compiled .NET commands
- **Functions:** PowerShell script-based commands
- **Variables:** Pre-defined data objects
- **Providers:** Access to specialized data stores
- **Workflows:** Complex automation sequences
- **Aliases:** Alternative command names

**Module Benefits:**
- **Functionality Extension:** Add specialized capabilities to PowerShell
- **Code Organization:** Logical grouping of related functionality
- **Sharing and Distribution:** Easy deployment across multiple systems
- **Version Control:** Managed updates and compatibility

### Module Discovery and Management

**Core Module Cmdlets:**
- **Get-Module:** List installed and available modules
- **Import-Module:** Load modules into current session
- **Install-Module:** Download and install from repositories
- **Remove-Module:** Unload modules from current session
- **Update-Module:** Upgrade to newer versions

---

## Lab Exercises

### Exercise 1: Module Discovery and Inventory

**Objective:** Master module discovery techniques for understanding current capabilities and identifying available tools.

#### Basic Module Listing

**Current Session Modules:**
```powershell
# List modules imported in current session
Get-Module
```

**All Available Modules:**
```powershell
# List all available modules on system
Get-Module -ListAvailable
```

**Specific Module Discovery:**
```powershell
# List particular module with version information
Get-Module -ListAvailable <module_name>
```

#### Practical Task: AWS Tools Module Analysis

**Task 1: Check AWS Tools Common Module**
```powershell
# List the AWS.Tools.Common module
Get-Module -ListAvailable AWS.Tools.Common
```

![AWS Tools Common Version](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/aws-tools-common-version.png)

**Question:** What is the installed version of the AWS.Tools.Common module?

**✅ Expected Answer:** 4.1.618

#### Security Module Discovery

**Common Security Modules:**
```powershell
# Find security-related modules
Get-Module -ListAvailable | Where-Object {$_.Name -like "*Security*"}

# Check for Active Directory module
Get-Module -ListAvailable ActiveDirectory

# Look for Windows Defender modules
Get-Module -ListAvailable | Where-Object {$_.Name -like "*Defender*"}
```

**SOC Applications:**
- **Capability Assessment:** Understand available security tools
- **Version Management:** Track module versions for consistency
- **Gap Analysis:** Identify missing security modules

---

### Exercise 2: Module Import and PSModulePath

**Objective:** Understand PowerShell module loading mechanisms and the PSModulePath environment variable.

#### Automatic Module Loading

**PowerShell Automatic Import:**
PowerShell automatically imports modules when you execute a command from an available module. This works when modules are installed in directories specified in the **PSModulePath** environment variable.

#### PSModulePath Environment Variable

**View Module Paths:**
```powershell
# Display PSModulePath directories
$env:PSModulePath -split ';'
```

![PSModulePath Environment Variable](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/psmodulepath-environment-variable.png)

**Standard Module Directories:**
- **System-wide:** `C:\Program Files\WindowsPowerShell\Modules`
- **User-specific:** `C:\Users\[Username]\Documents\WindowsPowerShell\Modules`
- **PowerShell Installation:** `C:\Windows\System32\WindowsPowerShell\v1.0\Modules`

#### Manual Module Import

**Import-Module Usage:**
```powershell
# Manually import specific module
Import-Module -Name ModuleName

# Import module from specific path
Import-Module -Name "C:\CustomModules\SecurityTools"

# Force reload of module
Import-Module -Name ModuleName -Force
```

**Question:** Which environment variable defines directory paths for PowerShell modules?

**✅ Expected Answer:** PSModulePath

**Security Applications:**
- **Custom Tool Integration:** Load proprietary security modules
- **Path Management:** Organize security tools in standard locations
- **Session Control:** Manage which security capabilities are available

---

### Exercise 3: PowerShell Gallery Module Installation

**Objective:** Master automated module installation from PowerShell Gallery for rapid capability deployment.

#### PowerShell Gallery Overview

**Repository Capabilities:**
PowerShell Gallery is a repository containing:
- **PowerShell Modules:** Packaged functionality
- **Scripts:** Standalone PowerShell code
- **DSC Resources:** Desired State Configuration components

**Gallery Benefits:**
- **Centralized Distribution:** Single source for community modules
- **Version Management:** Automatic handling of dependencies and updates
- **Security Scanning:** Microsoft-verified packages
- **Easy Installation:** Single-command deployment

#### Module Installation Process

**Basic Installation:**
```powershell
# Install module from PowerShell Gallery
Install-Module -Name <module_name>
```

**Installation Options:**
```powershell
# Install for current user only
Install-Module -Name ModuleName -Scope CurrentUser

# Install specific version
Install-Module -Name ModuleName -RequiredVersion "1.2.3"

# Install without confirmation prompts
Install-Module -Name ModuleName -Force
```

#### Practical Task: AWS Tools Installer

**Task 1: Install AWS Tools Installer Module**
```powershell
# Install AWS.Tools.Installer module from PowerShell Gallery
Install-Module -Name AWS.Tools.Installer
```

![Install AWS Tools Installer](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/install-aws-tools-installer.png)

**Note:** If a confirmation prompt appears regarding the untrusted repository, respond **Y** to trust the repository and proceed.

**Task 2: Verify Installation**
```powershell
# Verify module installation
Get-Module -ListAvailable AWS.Tools.Installer
```

![Get Module AWS Tools Installer](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/get-module-aws-tools-installer.png)

#### Security Considerations

**Repository Trust:**
- **Official Repository:** PowerShell Gallery is Microsoft-maintained
- **Publisher Verification:** Check module publisher and digital signatures
- **Community Reviews:** Review ratings and usage statistics
- **Source Code:** Many modules provide GitHub links for code review

**Installation Best Practices:**
- **Scope Selection:** Use -Scope CurrentUser for non-administrative installs
- **Version Pinning:** Specify exact versions for production environments
- **Dependency Management:** Understand module dependencies and conflicts

**SOC Applications:**
- **Rapid Deployment:** Quick installation of security analysis tools
- **Cloud Integration:** AWS, Azure, GCP security modules
- **Threat Intelligence:** Integration with TI platforms and APIs

---

### Exercise 4: Manual Module Installation

**Objective:** Implement manual module installation for custom security tools and specialized packages.

#### Manual Installation Scenarios

**When to Use Manual Installation:**
- **Custom Security Tools:** Proprietary or organization-specific modules
- **Offline Environments:** Air-gapped networks without internet access
- **Beta/Development Modules:** Pre-release security tools
- **Legacy Compatibility:** Older modules not available in Gallery

#### Installation Process

**Basic Manual Installation:**
```powershell
# Copy module to PowerShell modules directory
Copy-Item -Path <source_path> -Destination <psmodule_path> -Recurse
```

**Standard Destination Paths:**
- **User Modules:** `C:\Users\[Username]\Documents\WindowsPowerShell\Modules`
- **System Modules:** `C:\Program Files\WindowsPowerShell\Modules`

#### Practical Task: DsInternals Module

**Task 1: Manual Installation**
```powershell
# Install DsInternals module manually
Copy-Item -Path C:\Users\ComtechAdmin\Desktop\dsinternals -Destination C:\Users\ComtechAdmin\Documents\WindowsPowerShell\Modules -Recurse
```

![Copy Item DsInternals Manual](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/copy-item-dsinternals-manual.png)

**Task 2: Verify Manual Installation**
```powershell
# Verify DsInternals module installation
Get-Module -ListAvailable DsInternals
```

![Get Module DsInternals Verification](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/get-module-dsinternals-verification.png)

#### Post-Installation Validation

**Module Structure Verification:**
```powershell
# Check module manifest and files
Get-ChildItem "C:\Users\[Username]\Documents\WindowsPowerShell\Modules\DsInternals" -Recurse

# Test module import
Import-Module DsInternals -Verbose
```

**Security Applications:**
- **Custom Forensic Tools:** Install specialized investigation modules
- **Internal Security Tools:** Deploy organization-developed security modules
- **Offline Deployment:** Install modules in isolated security environments

---

### Exercise 5: Module Command Discovery

**Objective:** Explore module capabilities using Get-Command for comprehensive feature discovery and utilization.

#### Command Discovery Techniques

**Basic Command Listing:**
```powershell
# List all commands in specific module
Get-Command -Module <module_name>
```

**Command Filtering:**
```powershell
# Find commands by verb
Get-Command -Module ModuleName -Verb Get

# Find commands by noun
Get-Command -Module ModuleName -Noun User

# Find commands by pattern
Get-Command -Module ModuleName | Where-Object {$_.Name -like "*Security*"}
```

#### Practical Task: AWS Tools Installer Commands

**Task 1: Discover Available Commands**
```powershell
# List all available commands for AWS.Tools.Installer module
Get-Command -Module AWS.Tools.Installer
```

![Get Command AWS Tools Installer](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/get-command-aws-tools-installer.png)

**Question:** Which of the following commands are included in the AWS.Tools.Installer module?
- Install-AWSToolsModule
- Uninstall-AWSToolsModule  
- Update-AWSToolsModule
- Remove-AWSToolsModule
- Search-AWSToolsModule

**✅ Expected Answer:** Install-AWSToolsModule, Uninstall-AWSToolsModule, Update-AWSToolsModule

#### Advanced Command Analysis

**Command Details:**
```powershell
# Get detailed information about specific command
Get-Command Install-AWSToolsModule | Format-List *

# View command help
Get-Help Install-AWSToolsModule -Examples

# Check command parameters
(Get-Command Install-AWSToolsModule).Parameters.Keys
```

**Module Capability Assessment:**
```powershell
# Count total commands in module
(Get-Command -Module AWS.Tools.Installer).Count

# Group commands by verb
Get-Command -Module AWS.Tools.Installer | Group-Object Verb

# Find security-related commands
Get-Command -Module AWS.Tools.Installer | Where-Object {$_.Name -like "*Security*"}
```

**SOC Applications:**
- **Tool Discovery:** Understand available security functions
- **Capability Planning:** Assess module suitability for specific tasks
- **Training Development:** Create documentation for security team usage

---

## Advanced Module Management

### Module Version Control

**Version Management Commands:**
```powershell
# Check installed versions
Get-InstalledModule

# Update specific module
Update-Module -Name ModuleName

# Install specific version
Install-Module -Name ModuleName -RequiredVersion "2.1.0"

# Remove old versions
Uninstall-Module -Name ModuleName -RequiredVersion "1.0.0"
```

### Module Dependencies

**Dependency Analysis:**
```powershell
# View module dependencies
(Get-Module -ListAvailable ModuleName).RequiredModules

# Check dependency conflicts
Find-Module -Name ModuleName -IncludeDependencies
```

### Security Module Ecosystem

**Essential Security Modules for SOC:**
```powershell
# Active Directory analysis
Install-Module -Name ActiveDirectory

# Exchange investigation
Install-Module -Name ExchangeOnlineManagement

# Azure security
Install-Module -Name Az.Security

# Microsoft 365 security
Install-Module -Name Microsoft.Graph.Security

# Windows Event Log analysis
# (Built-in with Windows)
Get-Module -ListAvailable | Where-Object {$_.Name -like "*EventLog*"}
```

---

## Knowledge Assessment

### Module Discovery Mastery

**Question 1:** What command lists all available modules on the system?

**✅ Answer:** Get-Module -ListAvailable

**Question 2:** How do you check the version of a specific module?

**✅ Answer:** Get-Module -ListAvailable [ModuleName]

### Module Installation Expertise

**Question 1:** What is the standard command to install modules from PowerShell Gallery?

**✅ Answer:** Install-Module -Name [ModuleName]

**Question 2:** Which parameter installs a module for the current user only?

**✅ Answer:** -Scope CurrentUser

### Module Management Proficiency

**Question 1:** What environment variable defines PowerShell module search paths?

**✅ Answer:** PSModulePath

**Question 2:** How do you list all commands available in a specific module?

**✅ Answer:** Get-Command -Module [ModuleName]

---

## SOC Analyst Applications

### Daily Operations

**Security Module Inventory:**
```powershell
# Daily security module check
Get-Module -ListAvailable | Where-Object {
    $_.Name -like "*Security*" -or 
    $_.Name -like "*Defender*" -or 
    $_.Name -like "*AD*" -or
    $_.Name -like "*Exchange*"
} | Select-Object Name, Version, Path
```

**AWS Security Analysis:**
```powershell
# Install AWS security tools
Install-Module -Name AWS.Tools.SecurityHub
Install-Module -Name AWS.Tools.GuardDuty
Install-Module -Name AWS.Tools.Inspector

# Check AWS security posture
Import-Module AWS.Tools.SecurityHub
Get-SHUBFinding -MaxItem 50
```

### Incident Response

**Rapid Tool Deployment:**
```powershell
# Emergency security tool installation
Install-Module -Name Microsoft.Graph.Security -Force
Install-Module -Name ExchangeOnlineManagement -Force

# Quick capability assessment
Get-Command -Module Microsoft.Graph.Security | Where-Object {$_.Name -like "*Alert*"}
```

**Custom Investigation Tools:**
```powershell
# Deploy custom forensic modules
Copy-Item -Path "\\NetworkShare\SecurityTools\ForensicsModule" -Destination "$env:USERPROFILE\Documents\WindowsPowerShell\Modules" -Recurse

# Verify deployment
Import-Module ForensicsModule
Get-Command -Module ForensicsModule
```

### Compliance and Reporting

**Module Compliance Check:**
```powershell
# Generate module inventory report
Get-Module -ListAvailable | 
Select-Object Name, Version, Path, @{Name="LastWrite";Expression={(Get-Item $_.Path).LastWriteTime}} |
Export-Csv -Path "ModuleInventory_$(Get-Date -Format 'yyyy-MM-dd').csv" -NoTypeInformation
```

---

## Lab Validation

### Technical Skills Verification
```powershell
# Verify module discovery skills
(Get-Module -ListAvailable).Count

# Verify installation capabilities
Get-InstalledModule | Measure-Object

# Verify command discovery
Get-Command -Module AWS.Tools.Installer | Measure-Object
```

### Practical Application Testing
```powershell
# Complex module management workflow
$SecurityModules = @("AWS.Tools.SecurityHub", "Microsoft.Graph.Security")
foreach ($Module in $SecurityModules) {
    if (!(Get-Module -ListAvailable $Module)) {
        Install-Module -Name $Module -Scope CurrentUser -Force
    }
    Import-Module $Module
    Write-Host "$Module commands: $((Get-Command -Module $Module).Count)"
}
```

---

## Key Takeaways

**Module Management Mastery:**
- **Discovery Techniques:** Comprehensive module inventory and capability assessment
- **Installation Methods:** Both automated and manual deployment strategies
- **Version Control:** Consistent module versioning across SOC environment
- **Command Exploration:** Thorough understanding of module capabilities

**Security Tool Integration:**
- **Cloud Platforms:** AWS, Azure, Microsoft 365 security module integration
- **Enterprise Tools:** Active Directory, Exchange, and Windows security modules
- **Custom Solutions:** Manual deployment of proprietary security tools
- **Rapid Deployment:** Quick installation of emergency response tools

**Professional Development:**
- **Ecosystem Understanding:** Comprehensive knowledge of PowerShell module architecture
- **Automation Skills:** Scripted module management and deployment
- **Problem Solving:** Ability to identify and deploy appropriate security tools
- **Documentation:** Clear module inventory and usage documentation

**Operational Excellence:**
- **Consistency:** Standardized module deployments across SOC workstations
- **Flexibility:** Ability to rapidly deploy new security capabilities
- **Maintenance:** Systematic approach to module updates and version control
- **Integration:** Seamless incorporation of third-party security tools

