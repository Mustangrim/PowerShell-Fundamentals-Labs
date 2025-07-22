# PowerShell Fundamentals Lab 06: Challenge

## Lab Overview

**Lab Focus:** Comprehensive practical assessment integrating all PowerShell fundamentals for real-world SOC scenarios  
**Difficulty:** Expert    
**Author:** Mykyta Palamarchuk  
**Role:** SOC Analyst | Windows Automation & Incident Response  
**Certification:** CompTIA Security+ (SY0-701)

### Environment Specifications
- **Platform:** Windows 10/11 with PowerShell 5.1+ and WinRM enabled
- **Administrative Privileges:** Required for firewall management and remote operations
- **Business Context:** Enterprise SOC environment with security hardening and compliance requirements
- **Technology Focus:** Integration of commands, filtering, remoting, and automation for security operations

---

## Learning Objectives

Upon completion of this challenge lab, participants will have demonstrated:

1. **Integrated skill application** combining commands, filtering, remoting, and data manipulation
2. **Security-focused problem solving** using PowerShell for real-world SOC scenarios
3. **Remote system management** for security configuration and compliance operations
4. **Complex pipeline construction** for data collection, processing, and reporting
5. **Independent troubleshooting** using PowerShell's built-in help and discovery systems

---

## Business Scenario

### SOC Security Hardening Challenge
As a senior SOC analyst, you've been tasked with conducting security hardening and compliance verification across the enterprise infrastructure. This involves cleaning up legacy security configurations, inventorying system features, and generating compliance reports. These tasks require integration of all PowerShell fundamentals learned throughout the course, applied in realistic security operations scenarios.

**Operational Requirements:**
- **Security Hardening:** Remove outdated and insecure firewall rules
- **Compliance Reporting:** Generate detailed system configuration inventories
- **Remote Operations:** Execute all tasks against distributed infrastructure
- **Documentation:** Produce formatted reports for security compliance audits

---

## Challenge Framework

### Self-Directed Learning Approach

**Resource Utilization:**
This challenge emphasizes independent problem-solving using PowerShell's built-in resources:

```powershell
# Command discovery when unsure of exact cmdlet
Get-Command -Noun Firewall
Get-Command *WindowsFeature*

# Detailed help and examples
Get-Help Remove-NetFirewallRule -Examples
Get-Help Get-WindowsFeature -Detailed
```

**Problem-Solving Methodology:**
1. **Analyze Requirements:** Break down task objectives
2. **Discover Commands:** Use Get-Command for relevant cmdlets
3. **Research Syntax:** Leverage Get-Help for proper usage
4. **Test Incrementally:** Build pipeline step by step
5. **Validate Results:** Confirm expected outcomes

### Integration Skills Assessment

**Core Competencies Tested:**
- **Remote Execution:** PowerShell remoting for distributed operations
- **Data Filtering:** Where-Object for precise data selection
- **Object Manipulation:** Select-Object for property extraction
- **Data Sorting:** Sort-Object for organized output
- **Output Formatting:** Format-Table and Out-File for reporting
- **Pipeline Construction:** Complex multi-stage data processing

---

## Challenge Tasks

### Task 1: Security Firewall Rule Cleanup

**Scenario:** During a security audit of the server infrastructure, you've identified legacy firewall rules that pose security risks. Specifically, there's a rule allowing inbound Telnet connections on port 23. Since Telnet is an insecure protocol that transmits credentials in plaintext, this rule must be removed immediately.

**Security Context:**
- **Telnet Protocol:** Unencrypted communication protocol
- **Port 23:** Standard Telnet service port
- **Security Risk:** Credential exposure and unencrypted data transmission
- **Remediation:** Complete rule removal from firewall configuration

#### Task Requirements

**Objective:** Remove the firewall rule allowing Telnet connections on the server machine.

**Technical Steps:**
1. **Identify Target Rule:** Locate firewall rule related to Telnet
2. **Verify Rule Details:** Confirm rule purpose and scope
3. **Execute Removal:** Safely remove the identified rule
4. **Validate Change:** Confirm successful rule deletion

#### Solution Approach

**Step 1: Discover Telnet Firewall Rule**
```powershell
# Search for Telnet-related firewall rules on remote server
Invoke-Command -ComputerName server -ScriptBlock {
    Get-NetFirewallRule | Where-Object { $_.DisplayName -like "*telnet*" }
}
```

![Find Telnet Firewall Rule Server](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/find-telnet-firewall-rule-server.png)

**Step 2: Remove Identified Rule**
```powershell
# Remove the Telnet firewall rule
Invoke-Command -ComputerName server -ScriptBlock {
    Remove-NetFirewallRule -Name "FoundRuleName"
}
```

![Remove Telnet Firewall Rule](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/remove-telnet-firewall-rule.png)

#### Security Validation

**Post-Removal Verification:**
```powershell
# Confirm rule deletion
Invoke-Command -ComputerName server -ScriptBlock {
    Get-NetFirewallRule | Where-Object { $_.DisplayName -like "*telnet*" }
}
# Should return no results
```

**Impact Assessment:**
- **Security Posture:** Improved by removing insecure protocol access
- **Attack Surface:** Reduced by eliminating unnecessary network exposure
- **Compliance:** Aligned with security hardening standards

---

### Task 2: Windows Features Compliance Inventory

**Scenario:** The compliance team requires a comprehensive inventory of installed Windows features across all servers for audit purposes. This inventory must be properly formatted, sorted, and saved to a file for inclusion in the compliance report.

**Compliance Context:**
- **Audit Requirement:** Complete feature installation documentation
- **Standardization:** Consistent formatting across all reports
- **Verification:** Proof of current system configuration state
- **Documentation:** Permanent record for compliance archives

#### Task Requirements

**Comprehensive Pipeline Objectives:**
1. **Remote Data Collection:** Query Windows features on remote server
2. **Data Filtering:** Select only installed features
3. **Property Selection:** Extract Name and InstallState columns only
4. **Data Organization:** Sort alphabetically by feature name
5. **Format Standardization:** Ensure table format output
6. **Local Storage:** Save results to C:\features.txt on desktop machine

#### Solution Construction

**Step 1: Remote Feature Discovery**
```powershell
# Query installed Windows features on remote server
Invoke-Command -ComputerName server -ScriptBlock {
    Get-WindowsFeature | Where-Object -Property InstallState -eq "Installed"
}
```

![Get Windows Feature Installed](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/get-windows-feature-installed.png)

**Step 2: Property Selection**
```powershell
# Extract only Name and InstallState properties
Invoke-Command -ComputerName server -ScriptBlock {
    Get-WindowsFeature | Where-Object -Property InstallState -eq "Installed"
} | Select-Object Name, InstallState
```

![Select Name InstallState Columns](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/select-name-installstate-columns.png)

**Step 3: Alphabetical Sorting**
```powershell
# Sort features alphabetically by name
Invoke-Command -ComputerName server -ScriptBlock {
    Get-WindowsFeature | Where-Object -Property InstallState -eq "Installed"
} | Select-Object Name, InstallState | Sort-Object Name
```

![Sort Features By Name](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/sort-features-by-name.png)

**Step 4: Complete Pipeline with File Output**
```powershell
# Complete compliance inventory pipeline
Invoke-Command -ComputerName server -ScriptBlock {
    Get-WindowsFeature | Where-Object -Property InstallState -eq "Installed"
} | Select-Object Name, InstallState | Sort-Object Name | Out-File C:\features.txt
```

![Save Features To File](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/save-features-to-file.png)

#### Pipeline Analysis

**Command Integration Demonstrated:**
- **Remote Execution:** Invoke-Command for distributed operations
- **Data Filtering:** Where-Object for precise selection criteria
- **Object Manipulation:** Select-Object for property extraction
- **Data Organization:** Sort-Object for alphabetical ordering
- **Output Management:** Out-File for persistent storage

**Professional Benefits:**
- **Compliance Documentation:** Automated audit trail generation
- **Standardization:** Consistent reporting format
- **Efficiency:** Single pipeline handles complex requirements
- **Reproducibility:** Repeatable process for regular audits

---

## Advanced Challenge Extensions

### Extended Security Analysis

**Comprehensive Firewall Audit:**
```powershell
# Complete firewall security assessment
$FirewallAudit = Invoke-Command -ComputerName server -ScriptBlock {
    Get-NetFirewallRule | Where-Object {
        $_.Enabled -eq $true -and $_.Direction -eq "Inbound"
    } | Select-Object DisplayName, LocalPort, RemoteAddress, Action |
    Sort-Object LocalPort
}

# Identify potentially risky rules
$RiskyRules = $FirewallAudit | Where-Object {
    $_.LocalPort -in @(21, 23, 53, 135, 139, 445) -and 
    $_.RemoteAddress -eq "Any"
}
```

**Security Feature Inventory:**
```powershell
# Security-focused feature analysis
$SecurityFeatures = Invoke-Command -ComputerName server -ScriptBlock {
    Get-WindowsFeature | Where-Object {
        $_.Name -like "*Security*" -or 
        $_.Name -like "*Defender*" -or
        $_.Name -like "*Firewall*"
    }
} | Select-Object Name, InstallState, Description |
Sort-Object InstallState, Name
```

### Compliance Automation

**Multi-Server Inventory:**
```powershell
# Scale inventory across multiple servers
$Servers = @("server1", "server2", "server3")
$ComplianceReport = foreach ($Server in $Servers) {
    Invoke-Command -ComputerName $Server -ScriptBlock {
        [PSCustomObject]@{
            ServerName = $env:COMPUTERNAME
            Timestamp = Get-Date
            InstalledFeatures = (Get-WindowsFeature | Where-Object {$_.InstallState -eq "Installed"}).Count
            SecurityFeatures = (Get-WindowsFeature | Where-Object {
                $_.InstallState -eq "Installed" -and $_.Name -like "*Security*"
            }).Count
        }
    }
}
```

---

## Knowledge Validation

### Comprehensive Skills Assessment

**Technical Competencies Verified:**
1. **Remote Command Execution:** Successful use of Invoke-Command across scenarios
2. **Data Filtering Proficiency:** Effective Where-Object implementation for complex criteria
3. **Pipeline Construction:** Multi-stage data processing workflows
4. **Object Manipulation:** Property selection and data transformation
5. **Output Management:** Formatted results and file operations

### Problem-Solving Validation

**Independent Learning Demonstrated:**
- **Command Discovery:** Effective use of Get-Command for unknown cmdlets
- **Help System Utilization:** Leveraging Get-Help for syntax and examples
- **Incremental Development:** Building complex solutions step by step
- **Error Resolution:** Troubleshooting and correcting issues independently

### Security Operations Readiness

**SOC Analyst Capabilities:**
- **Security Hardening:** Proactive removal of security vulnerabilities
- **Compliance Management:** Automated audit and documentation processes
- **Remote Administration:** Efficient distributed system management
- **Documentation Standards:** Professional reporting and record keeping

---

## Professional Development Outcomes

### Technical Mastery

**PowerShell Fundamentals Mastered:**
- **Command Structure:** Comprehensive understanding of verb-noun conventions
- **Pipeline Operations:** Expert-level data flow and transformation
- **Remote Execution:** Scalable distributed system management
- **Module Integration:** Effective use of security and system management modules

### Security Operations Excellence

**SOC Analyst Skills Achieved:**
- **Security Assessment:** Systematic evaluation of system configurations
- **Risk Mitigation:** Proactive identification and remediation of vulnerabilities
- **Compliance Operations:** Automated audit and reporting capabilities
- **Incident Response:** Rapid remote investigation and remediation skills

### Career Advancement

**Professional Capabilities:**
- **Automation Expertise:** Complex workflow development and implementation
- **Problem-Solving Skills:** Independent resolution of technical challenges
- **Documentation Standards:** Professional report generation and maintenance
- **Scalability Thinking:** Enterprise-level solution design and implementation

---

## Next Steps and Continued Learning

### Advanced PowerShell Development

**Recommended Learning Paths:**
- **PowerShell Scripting:** Advanced script development and error handling
- **DSC (Desired State Configuration):** Infrastructure as code implementation
- **PowerShell Classes:** Object-oriented programming concepts
- **Azure PowerShell:** Cloud platform management and automation

### Security Specialization

**SOC Career Development:**
- **Advanced Threat Hunting:** PowerShell-based hunting techniques
- **Incident Response Automation:** Complex IR workflow development
- **Compliance Automation:** Enterprise-scale compliance management
- **Custom Tool Development:** Security-specific PowerShell module creation

### Enterprise Integration

**Infrastructure Mastery:**
- **Active Directory Automation:** Large-scale identity management
- **Exchange Administration:** Email system management and security
- **System Center Integration:** Enterprise management platform utilization
- **Cloud Hybrid Management:** Multi-platform administration skills

---


- **Comprehensive Technical Mastery** of PowerShell fundamentals  
- **Security Operations Readiness** for SOC analyst responsibilities  
- **Independent Problem-Solving** using PowerShell resources  
- **Professional Documentation** standards and practices  
- **Enterprise Thinking** for scalable solution development  

**Skills Validated:**
- Remote command execution and session management
- Complex data filtering and pipeline construction
- Security configuration management and hardening
- Compliance reporting and documentation automation
- Integrated workflow development for real-world scenarios

