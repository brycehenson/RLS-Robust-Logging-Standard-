# SLOGS (structured logging standard)
Defining a standard for logging data to a json structure.
## design philosophy
- human and machine readable
- language agnostic
- robust to various use cases



## example log
example_log_20190402T141013.txt
here i show a line that is  (structured)[http://jsonviewer.stack.hu/] for human reading. % denotes comments
```json
{
  "time_iso": "2019-04-02T14:10:13.110+11:00", %mandatory with local time zone
  "time_posix": 1.55417461311E+9,%mandatory UTC time
  "level": "log", %mandatory, 'log','error','data','analysis' 
  "environment": { %optional for all but first entry of each log file
    "tier": "development", %'development','testing,'model','production'
    "architecture": "win64",
    "computer_name": "brycelap",
    "network_interfaces": [
      {
        "FriendlyName": "Ethernet",
        "Description": "Realtek PCIe GBE Family Controller",
        "MAC_address": "xx-xx-xx-xx-xx-xx",
        "IPv4_address": "xxx.xxx.xxx.xxx",
        "IPv6_address": "xxxx::xxxx:xxxx:xxxx:xxxx"
      },
      {
        "FriendlyName": "Wi-Fi",
        "Description": "Intel(R) Dual Band Wireless-AC 7260",
        "MAC_address": "xx-xx-xx-xx-xx-xx",
        "IPv4_address": "xx.xx.xx.xx",
        "IPv6_address": "xxxx::xxxx:xxxx:xxxx:xxxx"
      }
    ]
  },
  "operation": "demonstrating how to use SLOGS", %mandatory
  "parameters": { %optional, specify state/data parameters here
    "photodiode_power": 0.46658893504181043,
    "drive_voltage": 1.1508870929981723,
    "feedback_error": -0.53341106495818957,
    "feedback_loop_time": 0.080674726853994641
  }
}
```	
## Design Notes & Questions
- want to be suitable for 100Hz logging
- [x] what should the extension be 
  - decided on .slog
- [ ] for multi machine logging how to aviod name collisions of logs.
  - what about just computer name
    - its a pretty shit network that has duplicate computer names
	- what about the same VM image
  - mac
    - macs can hop
	- not very human readable
	- collision very unlikely
- am i missing important fields
  - needs device type
- how to designate that a file is done being written too
  - double newline
  - log entry saying closed file
    - will require some code to get the last entry
- how to open a new log
  - want to limit a given log size for faster reading
- how to seek to specific times
  - log should have a closing time so that the range can be established
    - eg. "unnamed_log__20200328T200148.543+1100_to_20200328T200223.977+1100.slog"
    - what is the rename time and use penalty
    - time cost about 1.5ms
	- if file is open then wont be able to move
	  - will need to build a copy and tag that file for deletion
	- provides a done writing functionality
  - could use the last line entry but this imposes a penalty by having to open every file
  
  
  

## Resources
- [structured logging](https://stackify.com/what-is-structured-logging-and-why-developers-need-it/)
