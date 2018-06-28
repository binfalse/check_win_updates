# Check Windows Updates

This VBscript checks if windows machines require an update (and/or a subsequent reboot).
Tested for Windows 7 and Windows 10.

## Check for Update

To check for updates, this script creates an update searcher of an `Microsoft.Update.Session` object and filters for updates, which are

* assigned to the machine (`IsAssigned=1`)
* are not hidden to the users (`IsHidden=0`)
* are not yet installed (`IsInstalled=0`)

In case of matches, it outputs the number of uninstalled updates and list the details as performance data.

## Check for Reboot

The check for required reboots is performed using an `Microsoft.Update.SystemInfo` object.

## Installation

The installation of course depends on your infrastructure.
For example, if your running your checks using the NSClient++, you may save the script as `C:\Program Files\NSClient++\scripts\check_win_updates.wsf` and then add something like that to your `C:\Program Files\NSClient++\nsclient.ini`:

    [/settings/external scripts/wrappings]
    wsf=cscript.exe //T:40 //NoLogo scripts\%SCRIPT% %ARGS%
    
    [/settings/external scripts/wrapped scripts]
    check_updates = check_win_updates.wsf

The timeout of 40 is primarily for Windows 7 systems, which actually need a lot of time to search through installed updates...
Also make sure that your call of the nrpe tool at the monitoring server waits long enough for the answer!

To properly parse the details about pending updates, you may also need to add

    [/settings/external scripts/scripts/default]
    ignore perfdata = true

This line tells the NSClient++ to not interprete the update details as performance data, and instead just forwards the details.

## LICENSE

	check_win_updates.wsf - Check for pending updates on Windows machines
	Copyright (C) 2017 Martin Scharm <https://binfalse.de/contact/>

	This program is free software: you can redistribute it and/or modify
	it under the terms of the GNU General Public License as published by
	the Free Software Foundation, either version 3 of the License, or
	(at your option) any later version.

	This program is distributed in the hope that it will be useful,
	but WITHOUT ANY WARRANTY; without even the implied warranty of
	MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
	GNU General Public License for more details.

	You should have received a copy of the GNU General Public License
	along with this program.  If not, see <http://www.gnu.org/licenses/>.


