check_hardware_raid
===================

This is a Nagios plugin for monitoring 3ware and Adaptec RAID arrays. It
depends on the availability of tw_cli (for 3ware controllers) and arcconf
(for Adaptec controllers) CLI tools.


Usage
-----

	Usage: check_hardware_raid [options] 3ware|adaptec

	Options:
	  -h, --help          show this help message and exit
	  -b, --bbu           check status of battery backup unit
	  -i cX|X, --id=cX|X  controller id (default: c0 for 3ware, 1 for adaptec)
	Usage: check_hardware_raid [options] 3ware|adaptec

Status of arrays is gathered from `tw_cli /c0 show` or `arcconf getconfig 1`
command outputs depending on the chosen controller type. The CLI tools should
be in the current PATH. The default controller id can be altered with -i. 
Status of battery backup unit is checked if the plugin is called with -b.

Controller type gets chosen automatically if the plugin is ran as
check_3ware_raid or check_adaptec_raid (e.g. if renamed or symlinked).


Reporting
---------

The plugin checks all available array units. If all of them are healthy or
verifying, OK state is reported. If any of them is rebuilding, WARNING state
is reported. In all other cases the plugin reports a CRITICAL state.
Additionally, if the BBU check fails CRITICAL state is reported regardless of
the array state.

Plugin output contains info about all array units and drives and optionally
about batter backup units.

Sample output for 3ware controllers:

	unit(s): #0 verifying #1 ok, drive(s): #0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23 ok, battery: ok 

Sample output for Adaptec controllers:

	unit(s): #0 optimal, drive(s): #3 hot spare #0,1,2 online, battery: optimal 
