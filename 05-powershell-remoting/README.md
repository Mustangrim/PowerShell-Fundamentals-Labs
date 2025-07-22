# PowerShell Fundamentals Lab 05: PowerShell Remoting

## Lab Overview

**Lab Focus:** Remote PowerShell execution, session management, and distributed system administration for SOC operations  
**Difficulty:** Advanced    
**Author:** Mykyta Palamarchuk  
**Role:** SOC Analyst | Windows Automation & Incident Response  
**Certification:** CompTIA Security+ (SY0-701)

### Environment Specifications
- **Platform:** Windows 10/11 with PowerShell 5.1+ and WinRM enabled
- **Administrative Privileges:** Required for remote execution and session management
- **Business Context:** Enterprise SOC environment with distributed endpoint management
- **Technology Focus:** Remote command execution, session lifecycle, and multi-system orchestration

---

## Learning Objectives

Upon completion of this lab, participants will be able to:

1. **Execute remote commands** using Invoke-Command for single and multiple systems
2. **Manage PowerShell sessions** for persistent remote connections and efficient operations
3. **Handle variables in remote contexts** using $Using scope and local/remote variable management
4. **Deploy scripts remotely** using -FilePath parameter for distributed automation
5. **Implement session lifecycle management** for scalable remote administration

---

## Business Scenario

### SOC Remote Operations Challenge
Modern SOC operations require the ability to rapidly investigate and respond to threats across distributed enterprise environments. PowerShell Remoting provides essential capabilities for remote system analysis, evidence collection, and incident response coordination. Mastering remote execution enables SOC analysts to efficiently manage hundreds of endpoints from a centralized location while maintaining security and operational efficiency.

**Operational Requirements:**
- **Remote Investigation:** Analyze compromised systems without physical access
- **Mass Deployment:** Execute security updates and configurations across multiple systems
- **Evidence Collection:** Gather forensic data from distributed endpoints
- **Session Management:** Maintain persistent connections for complex investigation workflows

---

## PowerShell Remoting Architecture

### WinRM Foundation

**Windows Remote Management (WinRM):**
PowerShell Remoting relies on WinRM protocol for secure remote command execution:

- **Protocol:** HTTP/HTTPS-based communication
- **Authentication:** Kerberos (domain) or NTLM (workgroup/cross-domain)
- **Encryption:** Built-in message encryption and integrity
- **Scalability:** One-to-one and one-to-many remote execution

### Authentication Methods

**Domain Environment (Recommended):**
- **Kerberos Authentication:** Mutual authentication without credential transmission
- **Active Directory Integration:** Automatic trust relationships
- **Security Benefits:** No credential exposure, delegation support

**Workgroup/Cross-Domain:**
1. **SSL Certificates:** HTTPS connections with server identity verification
2. **TrustedHosts:** Simple configuration but lower security (lab environment)

**Security Note:** This lab uses TrustedHosts configuration for simplicity. Production environments should use Kerberos authentication via Active Directory.

### Core Remoting Cmdlets

**Remote Execution:**
- **Invoke-Command:** Execute commands on remote systems
- **Enter-PSSession:** Interactive remote sessions
- **Exit-PSSession:** Close interactive sessions

**Session Management:**
- **New-PSSession:** Create persistent remote sessions
- **Get-PSSession:** List active sessions
- **Remove-PSSession:** Terminate and cleanup sessions

---

## Lab Exercises

### Exercise 1: Basic Remote Command Execution

**Objective:** Master fundamental remote command execution using Invoke-Command for single system operations.

#### Single System Remote Execution

**Basic Invoke-Command Syntax:**
```powershell
# Execute command on remote system
Invoke-Command -ComputerName <hostname> -ScriptBlock {<commands>}
```

**Command Structure:**
- **-ComputerName:** Target system identifier
- **-ScriptBlock:** PowerShell code to execute remotely
- **Execution Context:** Commands run in remote system context

#### Practical Task: Remote File Creation

**Task 1: Create File on Remote System**
```powershell
# Create file on remote01 system
Invoke-Command -ComputerName remote01 -ScriptBlock {New-Item -Type File -Path "C:\Users\ComtechAdmin\Desktop\start.txt"}
```

![Invoke Command Create File Remote01](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/invoke-command-create-file-remote01.png)

**Command Analysis:**
- **Remote Execution:** File created on remote01, not local system
- **Return Data:** Success confirmation and file object details
- **Security Context:** Executes with current user's remote permissions

#### SOC Applications

**Remote Evidence Collection:**
```powershell
# Collect system information from compromised host
Invoke-Command -ComputerName suspected-host -ScriptBlock {
    Get-Process | Where-Object {$_.Company -eq $null} | Select-Object Name, Id, Path
}
```

**Remote Registry Analysis:**
```powershell
# Check for persistence mechanisms
Invoke-Command -ComputerName target-system -ScriptBlock {
    Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run"
}
```

---

### Exercise 2: One-to-Many Remote Operations

**Objective:** Implement scalable remote operations across multiple systems for enterprise-wide SOC activities.

#### Multiple System Targeting

**Comma-Separated Hosts:**
```powershell
# Execute on multiple systems simultaneously
Invoke-Command -ComputerName host1,host2,host3 -ScriptBlock {<commands>}
```

**Variable-Based Host Lists:**
```powershell
# Scale to hundreds of systems
$hosts = Get-Content C:\hosts.txt
Invoke-Command -ComputerName $hosts -ScriptBlock {<commands>}
```

#### Practical Task: Multi-System Timezone Analysis

**Task 1: Gather Timezone Information**
```powershell
# Check timezone configuration on multiple systems
Invoke-Command -ComputerName remote01,remote02 -ScriptBlock {Get-TimeZone}
```

![Invoke Command Get TimeZone Multiple Hosts](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/invoke-command-get-timezone-multiple-hosts.png)

**Questions:**
1. **What timezone is set on the remote01 host? (Value of Id property)**
   **✅ Expected Answer:** Pacific Standard Time

2. **What timezone is set on the remote02 host? (Value of Id property)**
   **✅ Expected Answer:** New Zealand Standard Time

#### Enterprise SOC Scenarios

**Security Patch Verification:**
```powershell
# Check patch status across all domain controllers
$DomainControllers = Get-ADDomainController -Filter * | Select-Object -ExpandProperty Name
Invoke-Command -ComputerName $DomainControllers -ScriptBlock {
    Get-HotFix | Where-Object {$_.InstalledOn -gt (Get-Date).AddDays(-30)}
}
```

**Threat Hunting at Scale:**
```powershell
# Search for suspicious processes across all servers
$ServerList = Get-ADComputer -Filter {OperatingSystem -like "*Server*"} | Select-Object -ExpandProperty Name
Invoke-Command -ComputerName $ServerList -ScriptBlock {
    Get-Process | Where-Object {$_.ProcessName -like "*mimikatz*" -or $_.ProcessName -like "*cobalt*"}
}
```

---

### Exercise 3: Variable Scope Management

**Objective:** Master local and remote variable handling for complex remote operations and data passing.

#### Variable Scope Concepts

**Remote Variables:**
- Variables defined in ScriptBlock belong to remote system
- Access remote system's environment and local variables

**Local Variables:**
- Variables defined on local system
- Require special syntax to pass to remote commands

#### $Using Scope Modifier

**Local to Remote Variable Passing:**
```powershell
# Pass local variable to remote command
$localVar = "value"
Invoke-Command -ComputerName remote -ScriptBlock {Write-Host $Using:localVar}
```

#### Practical Task: Variable Scope Demonstration

**Task 1: Remote Environment Variable**
```powershell
# Display computer name from remote system
Invoke-Command -ComputerName remote02 -ScriptBlock {Write-Host $ENV:COMPUTERNAME}
```

![ComputerName Variable Remote](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/computername-variable-remote.png)

**Task 2: Local Environment Variable via $Using**
```powershell
# Display local computer name on remote system
Invoke-Command -ComputerName remote02 -ScriptBlock {Write-Host $Using:ENV:COMPUTERNAME}
```

![Using ComputerName Variable Local](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/using-computername-variable-local.png)

**Questions:**
1. **What is the value of the $ENV:COMPUTERNAME variable on the remote02 machine? (Result of the first command)**
   **✅ Expected Answer:** REMOTE02

2. **What is the value of the $Using:ENV:COMPUTERNAME variable on the remote02 machine? (Result of the second command)**
   **✅ Expected Answer:** WINDOWS10

#### Advanced Variable Techniques

**ArgumentList Parameter (Alternative):**
```powershell
# Alternative method for passing local variables
$localData = Get-Date
Invoke-Command -ComputerName remote -ArgumentList $localData -ScriptBlock {
    param($RemoteDate)
    Write-Host "Local date passed: $RemoteDate"
}
```

**SOC Investigation Applications:**
```powershell
# Pass IOCs to remote hunting script
$SuspiciousHashes = @("hash1", "hash2", "hash3")
Invoke-Command -ComputerName $TargetHosts -ScriptBlock {
    param($HashList)
    Get-ChildItem C:\ -Recurse | Get-FileHash | Where-Object {$_.Hash -in $Using:SuspiciousHashes}
}
```

---

### Exercise 4: Remote Script Execution

**Objective:** Deploy and execute PowerShell scripts remotely for complex automation and investigation workflows.

#### Script Deployment Methods

**Inline ScriptBlock (Simple):**
```powershell
Invoke-Command -ComputerName remote -ScriptBlock {
    Get-Process | Out-File processes.txt
    Get-Service | Out-File services.txt
}
```

**FilePath Parameter (Recommended):**
```powershell
# Execute local script on remote systems
Invoke-Command -ComputerName remote -FilePath C:\Scripts\investigation.ps1
```

#### Practical Task: Remote Script Execution

**Task 1: Execute Investigation Script**
```powershell
# Run script.ps1 on multiple systems
Invoke-Command -ComputerName remote01,remote02 -FilePath "C:\Users\ComtechAdmin\Desktop\script.ps1"
```

![Invoke Command Run Script FilePath](https://raw.githubusercontent.com/Mustangrim/PowerShell-Fundamentals-Labs/main/assets/screenshots/invoke-command-run-script-filepath.png)

**Questions:**
1. **What is the value retrieved from the remote01 machine?**
   **✅ Expected Answer:** 5ac033955ea4765c7097b270d53f1448

2. **What is the value retrieved from the remote02 machine?**
   **✅ Expected Answer:** be3053b85b66ec787707b22a6091c959

#### Enterprise Script Deployment

**Security Configuration Script:**
```powershell
# Deploy security baseline across all workstations
$Workstations = Get-ADComputer -Filter {OperatingSystem -like "*Windows 10*"}
Invoke-Command -ComputerName $Workstations.Name -FilePath "C:\Scripts\SecurityBaseline.ps1"
```

**Incident Response Collection:**
```powershell
# Deploy forensic collection script
$CompromisedHosts = @("host1", "host2", "host3")
Invoke-Command -ComputerName $CompromisedHosts -FilePath "C:\Scripts\ForensicCollection.ps1"
```

---

### Exercise 5: Session Management and Lifecycle

**Objective:** Master PowerShell session management for efficient remote operations and persistent connections.

#### Session Lifecycle Overview

**Session Workflow:**
1. **Create:** New-PSSession - Establish remote connections
2. **List:** Get-PSSession - View active sessions
3. **Enter:** Enter-PSSession - Interactive remote shell
4. **Execute:** Invoke-Command -Session - Run commands via existing session
5. **Disconnect:** Disconnect-PSSession - Suspend but preserve session
6. **Reconnect:** Connect-PSSession - Resume suspended session
7. **Remove:** Remove-PSSession - Terminate and cleanup

#### Session Management Commands

**Create Persistent Sessions:**
```powershell
# Create sessions to multiple systems
$sessions = New-PSSession -ComputerName remote01,remote02,remote03

# Create named session
$dc_session = New-PSSession -ComputerName dc01 -Name "DC_Management"
```

**List and Manage Sessions:**
```powershell
# View all active sessions
Get-PSSession

# Filter sessions by computer name
Get-PSSession | Where-Object {$_.ComputerName -like "*remote*"}
```

**Interactive Sessions:**
```powershell
# Enter interactive session
Enter-PSSession -ComputerName remote01

# Exit interactive session (returns to local)
Exit-PSSession
```

**Session-Based Command Execution:**
```powershell
# Execute commands via existing session (faster)
$session = Get-PSSession | Where-Object {$_.ComputerName -eq "remote01"}
Invoke-Command -Session $session -ScriptBlock {Get-Process}
```

#### Session Benefits

**Performance Advantages:**
- **Reduced Overhead:** Reuse existing connections
- **Faster Execution:** No connection setup/teardown
- **State Preservation:** Variables and modules persist between commands

**Operational Benefits:**
- **Parallel Operations:** Multiple concurrent sessions
- **Session Persistence:** Disconnect/reconnect capability
- **Resource Management:** Controlled connection pooling

#### Knowledge Assessment

**Question 1:** Which cmdlet is used to list sessions?
- List-PSSession
- Get-PSSession
- Enter-PSSession

**✅ Expected Answer:** Get-PSSession

**Question 2:** Which cmdlet is used to destroy an existing session?
- Exit-PSSession
- Disconnect-PSSession
- Remove-PSSession

**✅ Expected Answer:** Remove-PSSession

---

## Advanced Remoting Techniques

### Security Considerations

**Authentication Best Practices:**
- **Kerberos:** Use domain authentication when possible
- **Credential Management:** Secure credential storage and rotation
- **Least Privilege:** Minimal necessary permissions for remote operations
- **Audit Logging:** Comprehensive logging of remote activities

**Network Security:**
- **Firewall Rules:** Restrict WinRM access to authorized sources
- **SSL/TLS:** Encrypt communications in untrusted networks
- **Network Segmentation:** Isolate administrative networks

### Error Handling and Troubleshooting

**Common Issues:**
```powershell
# Test WinRM connectivity
Test-WSMan -ComputerName remote01

# Check WinRM service status
Get-Service WinRM

# Verify authentication
Test-NetConnection -ComputerName remote01 -Port 5985
```

**Error Recovery:**
```powershell
# Robust remote execution with error handling
try {
    Invoke-Command -ComputerName $TargetHost -ScriptBlock {
        # Remote operations
    } -ErrorAction Stop
} catch {
    Write-Warning "Failed to execute on $TargetHost : $($_.Exception.Message)"
}
```

### Performance Optimization

**Parallel Execution:**
```powershell
# Execute commands in parallel across multiple systems
$jobs = Invoke-Command -ComputerName $Hosts -ScriptBlock {
    # Long-running operation
} -AsJob

# Monitor job progress
Get-Job | Receive-Job
```

**Session Pooling:**
```powershell
# Maintain connection pool for frequent operations
$sessionPool = New-PSSession -ComputerName $FrequentTargets
# Use sessions repeatedly
foreach ($task in $Tasks) {
    Invoke-Command -Session $sessionPool -ScriptBlock $task
}
```

---

## SOC Analyst Applications

### Incident Response Workflows

**Initial Compromise Assessment:**
```powershell
# Rapid assessment of potentially compromised hosts
$SuspiciousHosts = @("host1", "host2", "host3")
$sessions = New-PSSession -ComputerName $SuspiciousHosts

# Collect key indicators
Invoke-Command -Session $sessions -ScriptBlock {
    @{
        ComputerName = $env:COMPUTERNAME
        RunningProcesses = Get-Process | Where-Object {$_.Company -eq $null} | Select-Object Name, Id, Path
        RecentLogons = Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4624} -MaxEvents 50
        NetworkConnections = Get-NetTCPConnection | Where-Object {$_.State -eq "Established"}
        StartupPrograms = Get-CimInstance -ClassName Win32_StartupCommand
    }
}
```

**Lateral Movement Detection:**
```powershell
# Hunt for lateral movement across domain controllers
$DCs = Get-ADDomainController -Filter * | Select-Object -ExpandProperty Name
Invoke-Command -ComputerName $DCs -ScriptBlock {
    Get-WinEvent -FilterHashtable @{
        LogName = 'Security'
        ID = 4624, 4625, 4648
        StartTime = (Get-Date).AddHours(-24)
    } | Where-Object {$_.Message -like "*NTLM*"}
}
```

### Compliance and Configuration Management

**Security Baseline Verification:**
```powershell
# Verify security configurations across all servers
$AllServers = Get-ADComputer -Filter {OperatingSystem -like "*Server*"} | Select-Object -ExpandProperty Name
$ComplianceResults = Invoke-Command -ComputerName $AllServers -ScriptBlock {
    @{
        ComputerName = $env:COMPUTERNAME
        FirewallStatus = Get-NetFirewallProfile | Select-Object Name, Enabled
        AntivirusStatus = Get-CimInstance -Namespace "root\SecurityCenter2" -ClassName AntiVirusProduct
        UpdateStatus = Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 10
        UserRights = Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System"
    }
}
```

### Threat Hunting Operations

**Memory Analysis Campaign:**
```powershell
# Search for memory injection indicators across endpoints
$Endpoints = Get-ADComputer -Filter * | Select-Object -ExpandProperty Name
Invoke-Command -ComputerName $Endpoints -FilePath "C:\Scripts\MemoryAnalysis.ps1" -AsJob

# Monitor results
Get-Job | Receive-Job | Where-Object {$_.SuspiciousActivity -eq $true}
```

---

## Lab Validation

### Technical Skills Verification
```powershell
# Verify remote execution capabilities
Test-WSMan -ComputerName remote01

# Verify session management
$testSession = New-PSSession -ComputerName remote01
Get-PSSession
Remove-PSSession $testSession
```

### Operational Readiness Assessment
```powershell
# Complex multi-system operation
$targetHosts = @("remote01", "remote02")
$sessions = New-PSSession -ComputerName $targetHosts

# Parallel data collection
$results = Invoke-Command -Session $sessions -ScriptBlock {
    @{
        Hostname = $env:COMPUTERNAME
        Uptime = (Get-Date) - (Get-CimInstance -ClassName Win32_OperatingSystem).LastBootUpTime
        ProcessCount = (Get-Process).Count
        ServiceCount = (Get-Service).Count
    }
}

# Cleanup
Remove-PSSession $sessions
```

---

## Key Takeaways

**Remote Execution Mastery:**
- **Single and Multi-System Operations:** Scalable command execution across enterprise environments
- **Variable Scope Management:** Effective handling of local and remote variables using $Using
- **Script Deployment:** Remote execution of complex automation and investigation scripts
- **Performance Optimization:** Efficient operations through session reuse and parallel execution

**Session Management Expertise:**
- **Lifecycle Understanding:** Complete session management from creation to cleanup
- **Persistent Connections:** Maintaining long-term remote connections for complex operations
- **Resource Management:** Efficient use of network and system resources
- **Error Handling:** Robust remote operations with proper exception management

**SOC Operational Excellence:**
- **Incident Response:** Rapid remote investigation and evidence collection capabilities
- **Threat Hunting:** Distributed hunting operations across enterprise infrastructure
- **Compliance Verification:** Automated configuration and security baseline validation
- **Mass Operations:** Efficient deployment and management of security controls

**Professional Development:**
- **Enterprise Thinking:** Understanding of large-scale remote management challenges
- **Security Awareness:** Proper authentication, authorization, and audit practices
- **Automation Skills:** Complex workflow development using PowerShell remoting
- **Troubleshooting Expertise:** Systematic approach to remote connectivity and execution issues
