# Intel® Movidius™ VPUs Setup Guide for use with Intel® Distribution of OpenVINO™ toolkit

## See Also:

ANDREY: Let's link to all 5 of the other HDDL-R docs here, as a list;

- Intel® Movidius™ VPUs Programming Guide for use with the Intel® Distribution of OpenVINO™
- Intel® Vision Accelerator Design with Intel® Movidius™ VPUs HAL Configuration Guide
- Intel® Vision Accelerator Design with Intel® Movidius™ VPUs Workload Distribution User Guide
- Intel® Vision Accelerator Design with Intel® Movidius™ VPUs Scheduler User Guide
- Intel® Vision Accelerator Design with Intel® Movidius™ VPUs Errata

The IEI Mustang-V100-MX8 is an OEM version of the Intel® Vision Accelerator Design with Intel® Movidius™ VPUs.
This guide assumes you have installed the [Mustang-V100-MX8](https://download.ieiworld.com/) and the [Intel® Distribution of OpenVINO™ Toolkit](https://software.intel.com/en-us/openvino-toolkit).

Instructions in this guide for configuring your accelerator include:
1.	Installing the required IEI* BSL reset software.
2.	Configuration settings for the `hddldaemon` service.

**NOTE**: This guide does not apply to Uzel* cards.

## IEI  Reset Software Installation

Using the IEI Mustang-V100-MX8 requires downloading and installing the most current software for your system.

Visit the [IEI Download Center](https://download.ieiworld.com/) for the most current software and documentation.<br>
Search for:     **Mustang-V100-MX8**

As of 8/13/2019, the following links were for the latest software:

-  [IEI Reset Software Download, Linux](https://dls.ieiworld.com/IEIWeb/PDC_APP/PLM/OWFP000225/Mustang-V100_Linux_Plugin_1.0.1.20190409.tar.gz)

-  [IEI Reset Software Download, Windows](https://dls.ieiworld.com/IEIWeb/PDC_APP/PLM/OWFP000225/Mustang-V100_win64_Plugin_1.0.1.20190409.7z)

Download the appropriate software for your system, decompress the downloaded archive, enter the newly created directory, and run the install script:

On **Linux***:
-  Run the `install.sh script` with `sudo`, or as `root`.

On **Windows***, do one of the following:<br>
-  **GUI**: Double-click `install.bat`
-  **CLI**: Open a console with administrator privileges, cd into the directory, and run `install.bat`.

## Mustang-V100-MX8 Service Configuration

The `hddldaemon` is a system service, a binary executable that is run to manage the computational workload on the board.  It’s a required abstraction layer that handles inference, graphics processing, and any type of computation that should be run on the VPUs (video processing units).  Depending on the board configuration, there will be 8 or 16 VPUs.

**NOTE**: Graphics and other specialized processing may require some custom development.

### Conventions Used in this Document
`<IE>` refers to the following default OpenVINO Inference Engine Directories:
-  **Linux:**	    `/opt/intel/openvino/inference_engine`
-  **Windows:**	    `C:\Program Files (x86)\IntelSWTools\openvino\inference_engine`

If you have installed OpenVINO in a different directory on your system, you will need to enter your unique directory path.

### Configuration File Location

`<IE>\external\hddl\config\hddl_service.config`

DAVID: See this non-existent reference document for complete details.

### Service Configuration File Settings
Below are some possible configuration options. <br>
**NOTE:**  After changing a configuration file, the `hddldaemon` must be restarted. 

### Recommended Settings:

`device_snapshot_mode`<br>
Changes the output of the `hddldaemon` to display a table with individual VPU statistics.

**Default Setting:**<br>	
  `"device_snapshot_mode":    "none"`

**Suggested Setting:**<br>	
  `"device_snapshot_mode":    "full"`

**Supported Settings:**
-  `none` (default)
-  `base`
-  `full`

`device_snapshot_style`

**Default Setting:**<br>	
  `"device_snapshot_style":    "table"`

**Recommended Setting:**<br>	
  `"device_snapshot_style":    "table"`<br>

The `table` setting presents labels on the left for each column and is recommended as easier to read.  <br>
The `tape` setting prints the labels in each column.

**Supported Settings:**
-  `tape`
-  `table` (default)

`user_group	`<br>	
Restricts the service to group members. 

**Recommended	setting depends on your unique system configuration.**

**Default Setting**	,<br>
  `“user_group”:    "users"`

The `hddldaemon` may be restricted to a privileged group of users.  The appropriate group will vary according to the local system configuration.

**Supported Settings:**<br>
Valid groups on the current system.  The `“users”` group is a default group that exists on Windows and most Linux distributions.


**Optional Recommended Settings:**

`"device_utilization" : “off”`<br>
This setting displays the percent of time each VPU is in use.  It appears in the `table` column when turned on, or if `“device_fps”` is turned on.

`“memory_usage” : “off”`<br>
This setting reports the amount of memory being used by each VPU.

`“max_cycle_switchout”: 3`<br>
Requires the squeeze scheduler.  This setting might speed up performance significantly, depending on the app.  

**NOTE:** This setting works in conjunction with: `max_task_number_switch_out`.

`"client_fps" : “off”`<br>
This setting reports the total FPS for the dispatching hddl_service (which will have one or more clients per app).

`debug_service`<br>
`“debug_service”: “false”`<br>	(default: `“true”`)

## Additional Resources

- [Intel Distribution of OpenVINO Toolkit home page](https://software.intel.com/en-us/openvino-toolkit)

- [Intel Distribution of OpenVINO Toolkit documentation](https://docs.openvinotoolkit.org)