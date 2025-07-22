# PowerShell Fundamentals Lab 01: Introduction to PowerShell

## Lab Overview

**Lab Focus:** PowerShell command-line interface fundamentals for SOC analysts and Windows security operations  
**Difficulty:** Foundation    
**Author:** Mykyta Palamarchuk  
**Role:** SOC Analyst | Windows Automation & Incident Response  
**Certification:** CompTIA Security+ (SY0-701)

### Environment Specifications
- **Platform:** Windows 10/11 with PowerShell 5.1+
- **Administrative Privileges:** Required for security-related commands
- **Business Context:** Enterprise SOC environment with Windows infrastructure
- **Technology Focus:** Command-line automation for security operations

---

## Learning Objectives

Upon completion of this lab, participants will be able to:

1. **Understand PowerShell philosophy** and differences from traditional command shells
2. **Navigate PowerShell command structure** including verb-noun syntax and aliases
3. **Utilize PowerShell help system** for command discovery and syntax reference
4. **Explore cmdlets and modules** relevant to security operations
5. **Apply fundamental PowerShell concepts** for SOC analyst workflows

---

## Business Scenario

### SOC Environment Challenge
You've recently joined the SOC team at an enterprise managing Windows-heavy infrastructure. PowerShell skills are essential for effective incident response, system monitoring, and security investigations in Windows environments.

**Operational Requirements:**
- **Incident Response:** Command-line investigation capabilities
- **Automation:** PowerShell-based security monitoring
- **System Analysis:** Process and service management
- **Compliance:** Automated security reporting

---

## PowerShell Foundation Concepts

### PowerShell vs Command Prompt (CMD)

**PowerShell Philosophy:**
- **Human-Readable Commands:** Uses verb-noun naming convention (Get-Process vs tasklist)
- **Object-Oriented Output:** Returns .NET objects instead of plain text
- **Comprehensive Help System:** Built-in documentation for every command
- **Pipeline Functionality:** Pass objects between commands efficiently

**Administrative Modes:**
- **Normal Mode:** Default user privileges (damage control)
- **Administrative Mode:** Full system access (Right-click → Run as Administrator)

### Command Structure and Aliases

**Verb-Noun Convention:**
```powershell
Get-Process     # Retrieve running processes
Set-Location    # Change directory (equivalent to 'cd')
Stop-Service    # Stop a Windows service
```

**Alias System:**
PowerShell provides aliases for easier typing and compatibility:
- **dir** (from CMD) → Get-ChildItem
- **ls** (from Unix) → Get-ChildItem  
- **cd** → Set-Location
- **echo** → Write-Output

**Case Insensitivity:**
PowerShell commands are case-insensitive:
```powershell
get-alias     # Works fine
GET-ALIAS     # Also works
Get-Alias     # Standard formatting
```

### Cmdlets and Modules

**Cmdlet Fundamentals:**
- **Definition:** Specialized .NET classes implementing specific functions
- **Pipeline Support:** Output objects can be piped to other cmdlets
- **Self-Documenting:** Verb-noun naming makes purpose clear

**Module Organization:**
- **Scripted Modules:** PowerShell code files (.psm1 extension)
- **Binary Modules:** Compiled .NET assemblies (.dll files)
- **Security Focus:** Modules like Defender provide security-specific cmdlets

### Help System Architecture

**Get-Help Features:**
- **Basic Information:** Syntax and functionality overview
- **Detailed Help:** Comprehensive parameter descriptions
- **Examples:** Real-world usage scenarios
- **Update Capability:** Keep help files current with Update-Help

---

## Lab Exercises

### Exercise 1: PowerShell Environment Setup

**Objective:** Verify PowerShell environment and understand administrative modes.

**Task 1: Open PowerShell**
1. Locate PowerShell icon on desktop
2. Open PowerShell (administrative mode by default in lab environment)
3. Verify PowerShell prompt is active

**Expected Result:** PowerShell console opens with administrative privileges

---

### Exercise 2: Command Aliases and Discovery

**Objective:** Master PowerShell alias system and command discovery techniques.

**Task 1: Alias Investigation**
```powershell
# Discover what cmdlet uses the 'echo' alias
Get-Alias echo
```

![Get-Alias Echo Result](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/get-alias-echo-result.png)

**Question:** Which PowerShell cmdlet has the alias 'echo'?

**Expected Answer:** Write-Output

**Task 2: Explore Alias System**
```powershell
# List all available aliases
Get-Alias

# Count total number of aliases
(Get-Alias).count
```

**Validation:** Verify alias discovery functionality works correctly

---

### Exercise 3: Module and Cmdlet Analysis

**Objective:** Understand PowerShell modules and cmdlet organization for security operations.

**Task 1: Module Exploration**
```powershell
# List imported modules in current session
Get-Module

# Explore specific security module
Get-Command -Module Defender
```

**Task 2: Cmdlet Counting**
```powershell
# Count cmdlets in Defender module
(Get-Command -Module Defender).count
```

![Defender Module Cmdlet Count](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/defender-module-cmdlet-count.png)

**Question:** How many cmdlets does the Defender module contain?

**Expected Answer:** 12

**Task 3: Security Module Discovery**
```powershell
# Find security-related modules
Get-Module -ListAvailable | Where-Object {$_.Name -like "*Security*"}
```

---

### Exercise 4: Help System Utilization

**Objective:** Master PowerShell's built-in help system for efficient command learning.

**Task 1: Help System Preparation**
```powershell
# Update help files (if needed)
Update-Help -ErrorAction SilentlyContinue
```

**Task 2: Command Help Exploration**
```powershell
# Get basic help for Invoke-WebRequest
Get-Help Invoke-WebRequest

# View examples for Invoke-WebRequest
Get-Help Invoke-WebRequest -Examples
```

![Invoke-WebRequest Help Examples](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/invoke-webrequest-help-examples.png)

**Question:** When you view the help with examples for the Invoke-WebRequest cmdlet, what does the first example show?

**Options:**
- Send a web request
- Get images for a webpage  
- Download a file
- Terminate a session

**Expected Answer:** Send a web request

**Task 3: Security-Focused Help**
```powershell
# Explore help for security cmdlets
Get-Help Get-WinEvent -Examples
Get-Help Get-Service -Detailed
```

---

## Knowledge Assessment

### Command Structure Understanding

**Question 1:** What is the correct PowerShell command to change directory?

**Answer:** Set-Location (native) or cd (alias)

**Question 2:** What naming convention do PowerShell cmdlets follow?

**Answer:** Verb-Noun convention (e.g., Get-Process, Stop-Service)

### Module and Help System

**Question 1:** What command lists all available aliases?

**Answer:** Get-Alias

**Question 2:** How do you get examples for a specific cmdlet?

**Answer:** Get-Help [CmdletName] -Examples

---

## Lab Validation

### Environment Verification
```powershell
# Verify PowerShell version
$PSVersionTable.PSVersion

# Check if running with administrative privileges
([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole] "Administrator")
```

### Command Functionality Testing
```powershell
# Test alias discovery
Get-Alias echo

# Test module exploration  
(Get-Command -Module Defender).count

# Test help system
Get-Help Invoke-WebRequest -Examples
```

---

## Key Takeaways

**PowerShell Fundamentals Mastered:**
- **Command Philosophy:** Object-oriented, human-readable approach
- **Alias System:** Shortcuts for common commands and cross-platform compatibility
- **Module Architecture:** Organized cmdlet collections for specific functionality
- **Help System:** Comprehensive built-in documentation

**SOC Analyst Applications:**
- **Command Discovery:** Quickly find security-related cmdlets
- **Help Utilization:** Learn new commands without external documentation
- **Module Exploration:** Identify available security tools and capabilities
- **Efficient Navigation:** Use aliases for faster command execution

**Next Steps:**
- **Practice:** Regular use of Get-Help for new cmdlets
- **Exploration:** Investigate security-specific modules (Defender, EventLog)
- **Preparation:** Foundation for advanced PowerShell security operations

