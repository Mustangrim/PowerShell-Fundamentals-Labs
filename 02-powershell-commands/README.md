# PowerShell Fundamentals Lab 02: PowerShell Commands

## Lab Overview

**Lab Focus:** PowerShell command structure, parameters, aliases, and help system for SOC operations  
**Difficulty:** Foundation    
**Author:** Mykyta Palamarchuk  
**Role:** SOC Analyst | Windows Automation & Incident Response  
**Certification:** CompTIA Security+ (SY0-701)

### Environment Specifications
- **Platform:** Windows 10/11 with PowerShell 5.1+
- **Administrative Privileges:** Required for security-related commands
- **Business Context:** Enterprise SOC environment with command-line process management
- **Technology Focus:** Command syntax, aliases, and help system utilization

---

## Learning Objectives

Upon completion of this lab, participants will be able to:

1. **Master cmdlet structure** including verb-noun naming convention and parameter usage
2. **Utilize PowerShell aliases** for efficient command execution and cross-platform compatibility
3. **Navigate the help system** using Get-Help with various parameters and flags
4. **Apply command discovery techniques** to find relevant security and process management cmdlets
5. **Implement practical filtering** using pipelines and Where-Object for process analysis

---

## Business Scenario

### SOC Process Management Challenge
As a SOC analyst, you need to efficiently manage and investigate Windows processes during incident response. PowerShell commands provide essential capabilities for process analysis, service management, and system investigation. Mastering command syntax, aliases, and help system ensures rapid response during security incidents.

**Operational Requirements:**
- **Process Investigation:** Identify and analyze running processes
- **Command Efficiency:** Use aliases for faster command execution
- **Documentation Access:** Quickly access help for unfamiliar cmdlets
- **Filtering Techniques:** Extract specific process information for analysis

---

## PowerShell Command Architecture

### Cmdlet Fundamentals

**Verb-Noun Convention:**
PowerShell cmdlets follow a consistent **Verb-Noun** naming structure that makes commands self-documenting:

```powershell
Get-Process     # Retrieve running processes
Stop-Process    # Terminate a process
Start-Service   # Start a Windows service
```

**Key Rules:**
- **Noun is always singular:** Get-Module (not Get-Modules)
- **Specialized .NET classes:** Each cmdlet implements specific functions
- **Self-documenting:** Command purpose clear from name

### Parameter Usage and Syntax

**Basic Parameter Syntax:**
```powershell
Example-Cmdlet -Parameter Value
```

**Parameter Discovery:**
- Type dash (-) and press Tab to cycle through available parameters
- Use quotes for complex inputs and special characters
- Simple single-word inputs don't require quotes

**Practical Example:**
```powershell
# Find all cmdlets with "Get" verb
Get-Command -Verb "Get"
```

![Get-Command Verb Get](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/get-command-verb-get.png)

### External Tool Integration

PowerShell can execute external tools and executable files:
```powershell
# Run any .exe file
notepad.exe
calc.exe
```

This capability enables integration with security tools and legacy applications in SOC environments.

---

## Lab Exercises

### Exercise 1: Command Discovery and Parameter Usage

**Objective:** Master PowerShell command discovery techniques using Get-Command with various parameters.

**Task 1: Process-Related Cmdlet Discovery**
```powershell
# Find all cmdlets with "Process" noun
Get-Command -Noun Process
```

![Get-Command Noun Process](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/get-command-noun-process.png)

**Task 2: Counting Cmdlets**
```powershell
# Count cmdlets using Measure-Object
Get-Command -Noun Process | Measure-Object
```

![Measure Object Process Count](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/measure-object-process-count.png)

**Question:** How many cmdlets are found if you run Get-Command with -Noun parameter set to "Process"?

**Expected Answer:** 5

**Validation:** Verify command discovery functionality and parameter usage.

---

### Exercise 2: Alias System Mastery

**Objective:** Understand PowerShell alias system for efficient command execution and cross-platform compatibility.

#### Alias Fundamentals

**Common Aliases:**
- **dir** (from CMD) → Get-ChildItem
- **ls** (from Unix) → Get-ChildItem  
- **gci** (PowerShell abbreviation) → Get-ChildItem
- **gal** → Get-Alias

#### Alias Discovery Techniques

**Task 1: Basic Alias Lookup**
```powershell
# List all available aliases
Get-Alias

# Find specific alias
Get-Alias dir
```

**Task 2: Wildcard Search**
```powershell
# Find aliases starting with 'a'
Get-Alias a*
```

**Task 3: Reverse Alias Lookup**
```powershell
# Find aliases for commands containing "process"
Get-Alias -Definition *Process*
```

![Get-Alias Definition Process](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/get-alias-definition-process.png)

#### Security-Focused Alias Discovery

**Task 4: Where-Object Alias Investigation**
```powershell
# Find aliases for Where-Object cmdlet
Get-Alias -Definition Where-Object
```

![Get-Alias Where-Object](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/get-alias-where-object.png)

**Question:** What single character can be used as an alias for the Where-Object cmdlet?

**Expected Answer:** ?

**Explanation:** The question mark (?) serves as a shorthand alias for Where-Object, enabling faster filtering operations in pipelines.

---

### Exercise 3: Help System Utilization

**Objective:** Master PowerShell's comprehensive help system for efficient learning and troubleshooting.

#### Help System Features

**Get-Help Parameters:**
- **Basic:** `Get-Help Get-Process`
- **-Examples:** Show usage examples
- **-Detailed:** Parameter descriptions with examples
- **-Full:** Complete documentation
- **-Online:** Open Microsoft documentation in browser
- **-ShowWindow:** Searchable popup window

#### Help System Preparation

**Task 1: Update Help Files**
```powershell
# Update help documentation (requires internet)
Update-Help -Force -ErrorAction SilentlyContinue
```

**Task 2: Basic Help Usage**
```powershell
# Get basic help for Get-Process
Get-Help Get-Process

# View examples
Get-Help Get-Process -Examples

# Detailed help with parameters
Get-Help Get-Process -Detailed
```

#### Help System Aliases

**Common Help Aliases:**
- **man** (from Unix shells) → Get-Help
- **help** (simple shorthand) → Get-Help

**Usage Examples:**
```powershell
# These commands are equivalent
Get-Help Get-Service
man Get-Service
help Get-Service
```

---

### Exercise 4: Practical Process Management

**Objective:** Apply command discovery, aliases, and help system knowledge for real-world process management tasks.

#### Process Analysis Workflow

**Task 1: Command Discovery**
```powershell
# Find process-related cmdlets
Get-Command -Noun Process
```

![Get-Command Noun Process List](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/get-command-noun-process-list.png)

**Task 2: Process Information Retrieval**
```powershell
# List all running processes
Get-Process
```

![Get-Process Full Output](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/get-process-full-output.png)

**Task 3: Specific Process Investigation**
```powershell
# Find File Explorer process with filtering
Get-Process | Where-Object { $_.Name -eq "explorer" }
```

![Get-Process Explorer Filtered](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/get-process-explorer-filtered.png)

#### SOC Analyst Applications

**Security Process Analysis:**
```powershell
# Find processes without company information (potentially suspicious)
Get-Process | Where-Object { $_.Company -eq $null }

# High memory usage processes
Get-Process | Sort-Object WorkingSet -Descending | Select-Object -First 10

# Process with specific name pattern
Get-Process | Where-Object { $_.Name -like "*cmd*" }
```

**Question:** What is the Process ID of the File Explorer process?

**Expected Answer:** Variable (depends on current system state)

**Note:** Process IDs change with each system restart and process restart, so the specific ID will vary.

---

## Knowledge Assessment

### Command Structure Understanding

**Question 1:** What naming convention do PowerShell cmdlets follow?

**Answer:** Verb-Noun convention (e.g., Get-Process, Stop-Service)

**Question 2:** How do you discover available parameters for a cmdlet?

**Answer:** Type dash (-) and press Tab key to cycle through parameters

### Alias System Mastery

**Question 1:** What is the alias for Get-Alias cmdlet?

**Answer:** gal

**Question 2:** How do you find aliases for a specific cmdlet?

**Answer:** Get-Alias -Definition [CmdletName]

### Help System Proficiency

**Question 1:** What parameter shows usage examples for a cmdlet?

**Answer:** -Examples

**Question 2:** What are two common aliases for Get-Help?

**Answer:** man and help

---

## Advanced Techniques for SOC Operations

### Command Chaining and Filtering

**Pipeline Operations:**
```powershell
# Chain commands for complex analysis
Get-Process | Where-Object { $_.CPU -gt 100 } | Sort-Object CPU -Descending
```

**Parameter Tab Completion:**
```powershell
# Use Tab completion for efficiency
Get-Command -<Tab>  # Cycles through available parameters
```

### Security-Focused Command Discovery

**Finding Security Cmdlets:**
```powershell
# Security-related commands
Get-Command *Security*
Get-Command *Event*
Get-Command *Log*
```

### Help System Integration

**Quick Reference Workflow:**
1. **Discover:** `Get-Command -Noun [Topic]`
2. **Learn:** `Get-Help [Cmdlet] -Examples`
3. **Apply:** Use cmdlet with discovered parameters

---

## Lab Validation

### Environment Verification
```powershell
# Verify PowerShell version
$PSVersionTable.PSVersion

# Test command discovery
(Get-Command -Noun Process).Count

# Verify help system
Get-Help Get-Process -Examples
```

### Practical Skills Testing
```powershell
# Test alias knowledge
Get-Alias -Definition Where-Object

# Test parameter usage
Get-Command -Verb Get | Measure-Object

# Test filtering capabilities
Get-Process | Where-Object { $_.Name -eq "explorer" }
```

---

## Key Takeaways

**Command Mastery Achieved:**
- **Verb-Noun Structure:** Consistent, self-documenting command naming
- **Parameter Usage:** Efficient parameter discovery and application
- **Alias System:** Cross-platform compatibility and efficiency shortcuts
- **Help Integration:** Comprehensive self-service documentation access

**SOC Analyst Applications:**
- **Process Management:** Efficient investigation and analysis techniques
- **Command Efficiency:** Faster response through alias usage
- **Self-Service Learning:** Independent skill development through help system
- **Filtering Expertise:** Targeted data extraction for security investigations

**Practical Skills Developed:**
- **Get-Command:** Cmdlet discovery and exploration
- **Get-Alias:** Alias management and reverse lookup
- **Get-Help:** Documentation access and learning
- **Pipeline Filtering:** Data processing and analysis

**Next Steps:**
- **Practice:** Regular use of discovered commands in SOC workflows
- **Exploration:** Investigate security-specific cmdlet modules
- **Integration:** Combine commands for complex analysis tasks

