# PowerShell Fundamentals Lab 03: Filtering and Formatting

## Lab Overview

**Lab Focus:** Data manipulation, filtering, and formatting techniques for SOC analysis and security operations  
**Difficulty:** Intermediate    
**Author:** Mykyta Palamarchuk  
**Role:** SOC Analyst | Windows Automation & Incident Response  
**Certification:** CompTIA Security+ (SY0-701)

### Environment Specifications
- **Platform:** Windows 10/11 with PowerShell 5.1+
- **Administrative Privileges:** Required for firewall and system analysis
- **Business Context:** Enterprise SOC environment with data analysis requirements
- **Technology Focus:** Pipeline operations, object manipulation, and security data filtering

---

## Learning Objectives

Upon completion of this lab, participants will be able to:

1. **Master object selection** using Select-Object for property extraction and data limitation
2. **Implement data sorting** with Sort-Object for organized output and analysis
3. **Apply grouping techniques** using Group-Object for data categorization and statistics
4. **Utilize advanced filtering** with Where-Object and comparison operators for precise data extraction
5. **Combine pipeline operations** for complex data manipulation workflows in security contexts

---

## Business Scenario

### SOC Data Analysis Challenge
As a SOC analyst, you regularly need to process large volumes of system data including processes, services, firewall rules, and event logs. Effective data filtering and formatting are essential for rapid threat identification, compliance reporting, and system analysis. PowerShell's object manipulation capabilities provide powerful tools for transforming raw system data into actionable security intelligence.

**Operational Requirements:**
- **Process Analysis:** Filter and sort process data for threat hunting
- **Firewall Management:** Analyze firewall rule configurations and statistics
- **Data Export:** Save filtered results for reporting and documentation
- **Performance Optimization:** Limit output to relevant data for efficient analysis

---

## PowerShell Data Manipulation Architecture

### Pipeline Philosophy

PowerShell's strength lies in its **object-oriented pipeline** where data flows as structured .NET objects rather than plain text. This enables powerful data manipulation without complex parsing:

```powershell
# Object pipeline example
Get-Process | Where-Object {$_.CPU -gt 100} | Sort-Object CPU -Descending | Select-Object Name, CPU
```

### Key Manipulation Cmdlets

**Core Data Processing Cmdlets:**
- **Select-Object:** Property selection and object limitation
- **Sort-Object:** Data ordering and organization
- **Group-Object:** Data categorization and aggregation
- **Where-Object:** Conditional filtering and data extraction
- **Measure-Object:** Statistical analysis and counting

---

## Lab Exercises

### Exercise 1: Object Selection and Property Extraction

**Objective:** Master Select-Object for extracting specific properties and limiting output size for efficient data analysis.

#### Select-Object Fundamentals

**Basic Property Selection:**
The Select-Object cmdlet allows precise control over which object properties are displayed and processed:

```powershell
# Basic property selection
Get-Service | Select-Object -Property Status, Name, DisplayName

# Single property extraction
Get-Service | Select-Object -Property DisplayName
```

**Output Limitation:**
Control result set size using -First and -Last parameters:

```powershell
# First 10 services
Get-Service | Select-Object -Property DisplayName -First 10

# Last 5 processes
Get-Process | Select-Object -Property Name -Last 5
```

#### Select-Object vs Format-Table

**Critical Distinction:**
- **Select-Object:** Creates new objects with selected properties (data manipulation)
- **Format-Table:** Controls presentation only (display formatting)

```powershell
# Select-Object creates new object
$selectedData = Get-Process | Select-Object Name, CPU

# Format-Table only changes display
Get-Process | Format-Table Name, CPU
```

#### Practical Task: Process ID Extraction

**Task 1: Extract Process IDs**
```powershell
# Get list of processes and select only the Id property
Get-Process | Select-Object -Property Id
```

**Expected Output:**
```
Id
--
4312
6044
6068
6408
6676
6940
7988
3212
3528
7132
508
584
6344
1360
580
6580
3292
```

**Task 2: Save Process IDs to File**
```powershell
# Save process IDs to file for documentation
Get-Process | Select-Object -Property Id | Out-File C:\IDs.txt
```

**Result:** This command creates a file named IDs.txt in the root of drive C, containing all process IDs.

**Security Applications:**
- **Process Monitoring:** Extract specific process attributes for analysis
- **Data Export:** Save filtered results for compliance reporting
- **Performance Analysis:** Limit data to relevant properties for efficiency

---

### Exercise 2: Data Sorting and Organization

**Objective:** Implement Sort-Object for organizing data by various criteria to facilitate analysis and threat identification.

#### Sorting Fundamentals

**Basic Sorting Operations:**
```powershell
# Ascending order (default)
Get-Service | Sort-Object -Property Status

# Descending order
Get-Process | Sort-Object -Property CPU -Descending

# Multiple property sorting
Get-Service | Sort-Object -Property Status, Name
```

#### Advanced Sorting Techniques

**Property-Based Organization:**
```powershell
# Sort services by status to group running/stopped
Get-Service | Sort-Object -Property Status

# Combine with Select-Object for clean output
Get-Service | Sort-Object -Property Status | Select-Object -Property Status, DisplayName
```

#### Practical Task: Process Handle Analysis

**Task 1: Sort Processes by Handle Count**
```powershell
# Sort processes by handle count in descending order
Get-Process | Sort-Object -Property Handles -Descending
```

**Expected Output:**
```
Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
   2460       0      192        104      82.52      4   0 System
   1888      74    44540      73004       4.05   3292   1 explorer
   1296      57    35592     102784      10.73   6044   1 chrome
   1242      22     5600      11620       1.22    748   0 lsass
   1009      60    45696      70044       1.67   4616   1 SearchUI
    882      27    37340      45220       4.42   1068   0 svchost
    842      16     5016       9344       1.30   1012   0 svchost
    841      19     7228      14836       0.86    888   0 svchost
    819      29    15888      42480       0.44   3820   1 WindowsInternal.ComposableShell.Experiences.TextInput.Inpu...
    784      48    32704      65764       3.77    580   1 dwm
```

**Task 2: Extract Process with Most Handles**
```powershell
# Get process with most handles
Get-Process | Sort-Object -Property Handles -Descending | Select-Object -First 1
```

**Expected Output:**
```
Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
   2457       0      192        104      82.53      4   0 System
```

**Task 3: Extract Only Process Name**
```powershell
# Get only the process name of top handle consumer
Get-Process | Sort-Object -Property Handles -Descending | Select-Object -First 1 | Select-Object -ExpandProperty ProcessName
```

**Expected Output:**
```
System
```

**Question:** Which process has the most handles? Input the value of the ProcessName property.

**Expected Answer:** System

**Security Applications:**
- **Resource Analysis:** Identify processes consuming excessive system resources
- **Threat Hunting:** Sort processes by unusual characteristics (high CPU, memory, handles)
- **Performance Monitoring:** Track resource utilization patterns

---

### Exercise 3: Data Grouping and Aggregation

**Objective:** Utilize Group-Object for categorizing data and generating statistical summaries essential for security analysis.

#### Grouping Fundamentals

**Basic Grouping Operations:**
```powershell
# Group services by status
Get-Service | Group-Object -Property Status

# Group processes by company
Get-Process | Group-Object -Property Company
```

**Statistical Analysis:**
Group-Object provides automatic counting and categorization:
- **Count:** Number of objects in each group
- **Name:** Grouping criteria value
- **Group:** Collection of objects in the group

#### Practical Task: Firewall Rule Analysis

**Task 1: List Firewall Rules**
```powershell
# Get all firewall rules
Get-NetFirewallRule
```

**Sample Output:**
```
Name                  : vm-monitoring-dcom
DisplayName           : Virtual Machine Monitoring (DCOM-In)
Description           : Allow DCOM traffic for remote Windows Management Instrumentation.
DisplayGroup          : Virtual Machine Monitoring
Group                 : @icsvc.dll,-700
Enabled               : False
Profile               : Any
Platform              : {}
Direction             : Inbound
Action                : Allow
EdgeTraversalPolicy   : Block
LooseSourceMapping    : False
LocalOnlyMapping      : False
```

**Task 2: Group Firewall Rules by Direction**
```powershell
# Group firewall rules by direction (Inbound/Outbound)
Get-NetFirewallRule | Group-Object -Property Direction
```

**Expected Output:**
```
Count Name     Group
----- ----     -----
  231 Inbound  {MSFT_NetFirewallRule (CreationClassName = "MSFT_FW_FirewallRule_vm-monitoring-dcom"...
  179 Outbound {MSFT_NetFirewallRule (CreationClassName = "MSFT_FW_FirewallRule_WiFiDirect-KM-Driv"...
```

**Task 3: Extract Clean Count Summary**
```powershell
# Get clean count summary
Get-NetFirewallRule | Group-Object -Property Direction | Select-Object Name, Count
```

**Expected Output:**
```
Name     Count
----     -----
Inbound    231
Outbound   179
```

**Questions:**
1. **How many inbound firewall rules are set on your machine?**
   **Expected Answer:** 231

2. **How many outbound firewall rules are set on your machine?**
   **Expected Answer:** 179

**Security Applications:**
- **Configuration Analysis:** Understand firewall rule distribution and policies
- **Compliance Reporting:** Generate statistical summaries for audits
- **Trend Analysis:** Group security events by various criteria for pattern identification

---

### Exercise 4: Advanced Filtering with Where-Object

**Objective:** Master Where-Object with comparison operators for precise data filtering essential in security investigations.

#### Comparison Operators

**Essential Filtering Operators:**
- **-eq:** Equals (exact match)
- **-lt:** Less than (numeric/date comparison)
- **-gt:** Greater than (numeric/date comparison)
- **-like:** Pattern matching with wildcards
- **-match:** Regular expression matching
- **-contains:** Collection membership testing

#### Basic Filtering Examples

**Service Status Filtering:**
```powershell
# Find stopped services
Get-Service | Where-Object -Property Status -eq "Stopped"

# Alternative syntax
Get-Service | Where-Object {$_.Status -eq "Stopped"}
```

**Pattern Matching:**
```powershell
# Services containing "Windows" in display name
Get-Service | Where-Object DisplayName -like "*Windows*"
```

#### Advanced Filtering Techniques

**Multiple Criteria:**
```powershell
# Processes with high CPU and memory usage
Get-Process | Where-Object {$_.CPU -gt 100 -and $_.WorkingSet -gt 50MB}
```

**Wildcard Patterns:**
```powershell
# Files with specific extensions
Get-ChildItem | Where-Object Name -like "*.log"
```

#### Practical Task: WiFi Firewall Rule Analysis

**Task 1: Filter WiFi-Related Rules**
```powershell
# Find firewall rules containing "WiFi" in name
Get-NetFirewallRule | Where-Object -Property Name -Like "*WiFi*"
```

**Expected Output:**
```
Name                  : WiFiDirect-KM-Driver-In-TCP
DisplayName           : WFD Driver-only (TCP-In)
Description           : Inbound rule for drivers to communicate over WFD (TCP-In)
DisplayGroup          : WLAN Service - WFD Services Kernel Mode Driver Rules
Group                 : @wlansvc.dll,-36865
Enabled               : True
Profile               : Any
Platform              : {}
Direction             : Inbound
Action                : Allow
EdgeTraversalPolicy   : Block
LooseSourceMapping    : False
LocalOnlyMapping      : False
```

**Task 2: Count WiFi Rules**
```powershell
# Count WiFi-related firewall rules
(Get-NetFirewallRule | Where-Object -Property Name -Like "*WiFi*").Count
```

**Expected Output:**
```
4
```

**Question:** How many firewall rules contain the word "WiFi" in their name?

**Expected Answer:** 4

#### Counting Techniques

**Two Methods for Object Counting:**
```powershell
# Method 1: Measure-Object
Get-Service | Where-Object Status -eq "Running" | Measure-Object

# Method 2: .Count property
(Get-Service | Where-Object Status -eq "Running").Count
```

**Security Applications:**
- **Threat Hunting:** Filter processes, services, or files by suspicious characteristics
- **Compliance Checking:** Identify systems or configurations not meeting security standards
- **Incident Investigation:** Extract specific events or artifacts based on IoCs

---

## Advanced Pipeline Combinations

### Complex Data Processing Workflows

**Multi-Stage Pipeline Example:**
```powershell
# Comprehensive process analysis workflow
Get-Process | 
Where-Object {$_.WorkingSet -gt 50MB} |
Sort-Object WorkingSet -Descending |
Select-Object Name, Id, WorkingSet, CPU |
Format-Table -AutoSize
```

**Security Investigation Pipeline:**
```powershell
# Suspicious process identification
Get-Process | 
Where-Object {$_.Company -eq $null -and $_.CPU -gt 10} |
Sort-Object CPU -Descending |
Select-Object ProcessName, Id, CPU, Path |
Export-Csv -Path "C:\SuspiciousProcesses.csv" -NoTypeInformation
```

### Performance Optimization

**Efficient Filtering Strategies:**
1. **Filter Early:** Use Where-Object before Sort-Object when possible
2. **Limit Properties:** Select only needed properties with Select-Object
3. **Restrict Output:** Use -First/-Last to limit result sets
4. **Cache Results:** Store filtered data in variables for reuse

```powershell
# Efficient approach
$criticalProcesses = Get-Process | Where-Object {$_.CPU -gt 100}
$topProcesses = $criticalProcesses | Sort-Object CPU -Descending | Select-Object -First 10
```

---

## Knowledge Assessment

### Object Selection Mastery

**Question 1:** What is the difference between Select-Object and Format-Table?

**Answer:** Select-Object creates new objects with selected properties (data manipulation), while Format-Table only controls display presentation without modifying the object.

**Question 2:** How do you limit output to the first 5 results?

**Answer:** Use Select-Object -First 5

### Sorting and Grouping

**Question 1:** What parameter sorts data in descending order?

**Answer:** -Descending

**Question 2:** Which cmdlet provides automatic counting when categorizing data?

**Answer:** Group-Object

### Filtering Expertise

**Question 1:** What operator is used for pattern matching with wildcards?

**Answer:** -like

**Question 2:** What are two methods for counting filtered objects?

**Answer:** Measure-Object and .Count property

---

## SOC Analyst Applications

### Daily Operations

**Process Monitoring:**
```powershell
# Daily process health check
Get-Process | 
Where-Object {$_.CPU -gt 50 -or $_.WorkingSet -gt 100MB} |
Sort-Object CPU -Descending |
Format-Table ProcessName, CPU, WorkingSet
```

**Service Status Review:**
```powershell
# Security service status check
Get-Service | 
Where-Object {$_.Name -like "*Security*" -or $_.Name -like "*Defender*"} |
Select-Object Name, Status, StartType |
Format-Table -AutoSize
```

**Firewall Configuration Analysis:**
```powershell
# Firewall rule audit
Get-NetFirewallRule | 
Where-Object {$_.Enabled -eq $true} |
Group-Object Direction, Action |
Select-Object Name, Count |
Sort-Object Count -Descending
```

### Incident Response

**Suspicious Activity Detection:**
```powershell
# Unsigned process detection
Get-Process | 
Where-Object {$_.Company -eq $null} |
Select-Object ProcessName, Id, Path, StartTime |
Export-Csv -Path "UnsignedProcesses.csv"
```

**Resource Abuse Investigation:**
```powershell
# High resource consumption analysis
Get-Process |
Where-Object {$_.CPU -gt 100 -or $_.WorkingSet -gt 500MB} |
Sort-Object @{Expression="CPU";Descending=$true}, @{Expression="WorkingSet";Descending=$true} |
Format-Table ProcessName, CPU, WorkingSet, Id
```

---

## Lab Validation

### Technical Skills Verification
```powershell
# Verify filtering capabilities
(Get-Process | Where-Object {$_.ProcessName -eq "System"}).Count

# Verify sorting functionality
Get-Service | Sort-Object Status | Select-Object -First 1

# Verify grouping operations
Get-NetFirewallRule | Group-Object Direction | Measure-Object
```

### Practical Application Testing
```powershell
# Complex pipeline validation
Get-Process | 
Where-Object {$_.WorkingSet -gt 10MB} |
Sort-Object WorkingSet -Descending |
Select-Object -First 5 |
Group-Object Company |
Select-Object Name, Count
```

---

## Key Takeaways

**Data Manipulation Mastery:**
- **Select-Object:** Property extraction and output control for focused analysis
- **Sort-Object:** Data organization for pattern identification and prioritization
- **Group-Object:** Statistical analysis and categorization for trend identification
- **Where-Object:** Precise filtering for targeted investigations and compliance checking

**Pipeline Proficiency:**
- **Object Flow:** Understanding data transformation through pipeline stages
- **Performance Optimization:** Efficient ordering of operations for better performance
- **Complex Workflows:** Combining multiple cmdlets for sophisticated analysis
- **Error Handling:** Graceful handling of pipeline failures and data issues

**Security Applications:**
- **Threat Hunting:** Systematic approach to identifying suspicious activities
- **Compliance Monitoring:** Automated checking of security configurations
- **Incident Response:** Rapid data analysis during security incidents
- **Reporting Automation:** Consistent generation of security reports and documentation

**Professional Development:**
- **Analytical Thinking:** Structured approach to data analysis problems
- **Tool Mastery:** Deep understanding of PowerShell's data manipulation capabilities
- **Efficiency Gains:** Significant time savings through automation
- **Documentation Skills:** Clear reporting and evidence preservation

