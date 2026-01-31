# NetFlowWin - Process Network Monitor

A powerful Windows application for monitoring network connections, registry changes, and file operations per process. Block or unblock Internet access using the native Windows Firewall.

**Version:** 1.0  
**Author:** Emmanuel Forgues  
**License:** MIT  
**Year:** 2026

---

## Features

### Real-Time Process Monitoring
- View all running processes in a hierarchical tree structure
- Monitor CPU and memory usage per process
- Track process start times and execution paths

### Network Tab
- Display all active TCP/UDP connections for each process
- Show local and remote IP addresses with ports
- Automatic hostname resolution (FQDN)
- Connection state tracking (Established, Listen, TimeWait, etc.)

### Registry Tab
- Real-time registry modification monitoring via ETW (Event Tracing for Windows)
- Track which registry keys are being read, written, or deleted
- Filter by process to focus on specific activity

### Files Tab
- Monitor file system operations (create, read, write, delete)
- Track file access patterns per process
- Identify processes modifying critical system files

### Timeline Tab
- Chronological view of all process activities
- Combine network, registry, and file events in a single timeline
- Useful for forensic analysis and incident investigation

### Firewall Control
- Block any process from accessing the network with a single click
- Temporary blocking options: 5 min, 30 min, 1 hour, 6 hours, 24 hours
- Permanent blocking until manual unblock
- Direction control: Inbound, Outbound, or Both
- Visual indication of blocked processes (red highlight)
- Rules managed via native Windows Firewall (netsh advfirewall)

### 3D Visualization
- Interactive 3D view of processes and their network connections
- Visualize process dependencies and communication flows
- Color coding: Green (normal), Red (blocked)
- Mouse controls: Rotate, Zoom, Pan

### AI Analysis (Ollama Integration)
- Connect to a local Ollama instance for AI-powered analysis
- Automatic detection of suspicious process behaviors
- Security reports with risk scores
- Action recommendations (Block, Monitor, Ignore)
- 100% local processing - no data sent externally
- Compatible with all Ollama models (llama3, mistral, codellama, etc.)

---

## System Requirements

- **Operating System:** Windows 10 or Windows 11 (64-bit)
- **Runtime:** .NET 8.0 (included - self-contained deployment)
- **Privileges:** Administrator rights required
- **Optional:** Ollama installed locally for AI analysis features

---

## Installation

No installation required. This is a portable, self-contained application.

1. Extract all files to a folder of your choice
2. Run `NetFlowWin.UI.exe` as Administrator

> **Important:** Administrator privileges are required for:
> - ETW (Event Tracing for Windows) monitoring
> - Windows Firewall rule management
> - Process information access

---

## Usage

### Starting the Application

1. Right-click on `NetFlowWin.UI.exe`
2. Select "Run as administrator"
3. The main window will display all running processes

### Monitoring a Process

1. Select a process from the tree view
2. Use the tabs to view different activity types:
   - **Network:** Active connections
   - **Registry:** Registry modifications
   - **Files:** File operations
   - **Timeline:** All events chronologically

### Blocking a Process

1. Select a process from the tree view
2. Click the **Block** button in the toolbar
3. Choose blocking options:
   - Duration (temporary or permanent)
   - Direction (Inbound/Outbound/Both)
4. Click **Block** to apply the firewall rule

### Unblocking a Process

1. Select a blocked process (highlighted in red)
2. Click the **Unblock** button in the toolbar
3. The firewall rule will be removed immediately

### Using AI Analysis

1. Ensure Ollama is running locally (default: http://localhost:11434)
2. Go to **Settings** > **Ollama Configuration**
3. Select your preferred model
4. Select a process and click **Analyze** for an AI security report

### 3D Visualization

1. Click the **3D View** button in the toolbar
2. Use mouse controls:
   - **Left drag:** Rotate view
   - **Scroll wheel:** Zoom in/out
   - **Right drag:** Pan view
3. Click on a node to select the corresponding process

---

## Configuration

### appsettings.json

The application configuration file supports the following options:

```json
{
  "Ollama": {
    "BaseUrl": "http://localhost:11434",
    "Model": "llama3",
    "TimeoutSeconds": 600
  },
  "Monitoring": {
    "RefreshIntervalMs": 1000,
    "EnableRegistryMonitoring": true,
    "EnableFileMonitoring": true
  },
  "Logging": {
    "Level": "Information",
    "FilePath": "logs/netflowwin.log"
  }
}
```

---

## Firewall Rules

NetFlowWin creates firewall rules with the following naming convention:

```
NetFlowWin_Block_<ProcessName>_<Direction>
```

Example: `NetFlowWin_Block_chrome_Out`

To view all NetFlowWin rules manually:
```powershell
netsh advfirewall firewall show rule name=all | findstr "NetFlowWin"
```

To remove all NetFlowWin rules manually:
```powershell
netsh advfirewall firewall delete rule name=all dir=out program="C:\path\to\process.exe"
```

---

## Troubleshooting

### Application won't start
- Ensure you're running as Administrator
- Check that all DLL files are present in the same folder as the executable

### No network connections displayed
- Verify the process has active connections
- Some connections may be too brief to capture

### Registry/File monitoring not working
- ETW requires Administrator privileges
- Restart the application with elevated rights

### Ollama analysis fails
- Verify Ollama is running: `ollama serve`
- Check the configured URL in settings
- Ensure the selected model is downloaded: `ollama pull llama3`

### Firewall rules not applying
- Run the application as Administrator
- Check Windows Firewall service is running
- Verify the process path is correct

---

## Technical Details

### Technologies Used
- **.NET 8.0** - Application framework
- **WPF** - User interface (Windows Presentation Foundation)
- **ETW** - Event Tracing for Windows for system monitoring
- **TraceEvent** - Microsoft library for ETW consumption
- **Entity Framework Core** - Data persistence with SQLite
- **CommunityToolkit.Mvvm** - MVVM pattern implementation
- **Serilog** - Logging framework
- **WPF 3D (Viewport3D)** - 3D visualization

### Architecture
```
NetFlowWin.UI          - WPF application layer (Views, ViewModels)
NetFlowWin.Core        - Business logic (Services, Models)
```

### Data Storage
- Process data and settings stored in SQLite database
- Logs stored in `logs/` subfolder
- No data sent to external servers

---

## Security Considerations

- All analysis is performed locally
- Network blocking uses native Windows Firewall
- No telemetry or external data collection
- Source code available for audit

---

## Known Limitations

- Windows only (no Linux/macOS support)
- Requires Administrator privileges
- Some system processes may not be blockable
- ETW events may be missed under extreme system load

---

## Changelog

### Version 1.0 (January 2026)
- Initial release
- Process monitoring with Network/Registry/Files/Timeline tabs
- Windows Firewall integration for blocking/unblocking
- 3D visualization of process network topology
- Ollama AI integration for security analysis
- Temporary and permanent blocking options

---

## Support

For issues, feature requests, or contributions, please visit the project repository.

---

## License

MIT License

Copyright (c) 2026 Emmanuel Forgues

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
