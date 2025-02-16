# Disable Microsoft Defender

Microsoft Defender for Endpoint is an enterprise endpoint security platform that helps defend against advanced persistent threats.

Your organisation may install and manage this via Microsoft Intune. Unfortunately, this well intentioned software can sometimes chew up valuable CPU resources.

## Check status

First of all, check the status of Microsoft Defender extensions:

```
% mdatp health --details system_extensions
network_extension_enabled                   : true             <--- ENABLED
network_extension_installed                 : true
endpoint_security_extension_ready           : true
endpoint_security_extension_installed       : true
```

To check anti-virus status:

```
mdatp health --details antivirus
passive_mode_enabled                        : false
real_time_protection_enabled                : true             <--- ENABLED
real_time_protection_available              : true
real_time_protection_subsystem              : "endpoint_security_extension"
network_events_subsystem                    : "network_filter_extension"
scan_archives                               : true
scan_after_definition_update                : true
scan_history_cleanup_interval_hours         : 7
scan_results_retention_days                 : 90
scan_history_maximum_items                  : 10000
enable_file_hash_computation                : false
last_on_demand_scan_start_time              : Dec 02, 2024 at 11:50:19 AM
last_on_demand_scan_end_time                : Dec 02, 2024 at 11:50:23 AM
last_on_demand_scan_type                    : "quick"
last_on_demand_scan_threat_count            : 0
last_on_demand_scan_files_scanned           : 5566
maximum_on_demand_scan_threads              : 2
enforcement_level                           : "on_demand"
```

## Disable Network Filtering

You can disable network filtering with this one-liner:

```
sudo mdatp system-extension network-filter disable
```

An extension status check will show that it has been disabled:

```
% mdatp health --details system_extensions
network_extension_enabled                   : false            <--- DISABLED
network_extension_installed                 : true
endpoint_security_extension_ready           : true
endpoint_security_extension_installed       : true
```

This can be re-enabled at any time:

```
sudo mdatp system-extension network-filter disable
```

## Disable Anti-virus (Real-time protection)

You can disable anti-virus real-time protection with this one-liner:

```
sudo mdatp config real-time-protection --value disabled
```

Check that it has been disabled:

```
mdatp health --details antivirus
passive_mode_enabled                        : false
real_time_protection_enabled                : false            <--- DISABLED
real_time_protection_available              : true
real_time_protection_subsystem              : "endpoint_security_extension"
network_events_subsystem                    : "network_filter_extension"
scan_archives                               : true
...
```

This can also be re-enabled at any time:

```
sudo mdatp config real-time-protection --value enabled
```
