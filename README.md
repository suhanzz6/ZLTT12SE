PS C:\Users\ishak> Get-PnpDevice -PresentOnly |
>> Where-Object {
>>     $_.InstanceId -match 'VID_19D2&PID_1557'
>> } |
>> Format-Table Status,Class,FriendlyName,InstanceId -AutoSize

Status Class FriendlyName                                 InstanceId
------ ----- ------------                                 ----------
OK     USB   USB Composite Device                         USB\VID_19D2&PID_1557\1234567890ABCDEF
OK     Net   Remote NDIS based Internet Sharing Device #3 USB\VID_19D2&PID_1557&MI_00\6&23391696&0&0000


PS C:\Users\ishak> Get-PnpDevice -PresentOnly |
>> Where-Object {
>>     $_.InstanceId -match 'VID_19D2&PID_1557&MI_'
>> } |
>> ForEach-Object {
>>     "`n--- $($_.FriendlyName) ---"
>>     Get-PnpDeviceProperty -InstanceId $_.InstanceId `
>>     -KeyName 'DEVPKEY_Device_Service'
>> }

--- Remote NDIS based Internet Sharing Device #3 ---

InstanceId KeyName                                   Type       Data
---------- -------                                   ----       ----
USB\VID... DEVPKEY_Device_Service                    String     usbrndis6


PS C:\Users\ishak> Get-PnpDeviceProperty `
>> -InstanceId 'USB\VID_19D2&PID_1557\1234567890ABCDEF' `
>> -KeyName 'DEVPKEY_Device_CompatibleIds',
>> 'DEVPKEY_Device_ContainerId',
>> 'DEVPKEY_Device_BusReportedDeviceDesc'

InstanceId KeyName                                   Type       Data
---------- -------                                   ----       ----
USB\VID... DEVPKEY_Device_CompatibleIds              StringList {USB\COMPAT_VID_19D2&DevClass_00&SubClass_00&Prot00,...
USB\VID... DEVPKEY_Device_ContainerId                Guid       {F2E6A928-059B-5B0D-A871-DF512735633E}
USB\VID... DEVPKEY_Device_BusReportedDeviceDesc      String     DEMO Mobile Boardband


PS C:\Users\ishak> Get-NetAdapter |
>> Where-Object {$_.InterfaceDescription -like '*Remote NDIS*'} |
>> Format-List Name,Status,MacAddress,InterfaceDescription


Name                 : Ethernet 5
Status               : Up
MacAddress           : 34-4B-50-00-00-00
InterfaceDescription : Remote NDIS based Internet Sharing Device #3



PS C:\Users\ishak> (Get-PnpDeviceProperty `
>> -InstanceId 'USB\VID_19D2&PID_1557\1234567890ABCDEF' `
>> -KeyName 'DEVPKEY_Device_CompatibleIds').Data |
>> ForEach-Object { $_ }
USB\COMPAT_VID_19D2&DevClass_00&SubClass_00&Prot00
USB\COMPAT_VID_19D2&DevClass_00&SubClass_00
USB\COMPAT_VID_19D2&DevClass_00
USB\DevClass_00&SubClass_00&Prot_00
USB\DevClass_00&SubClass_00
USB\DevClass_00
USB\COMPOSITE
PS C:\Users\ishak> Get-NetIPConfiguration -InterfaceAlias "Ethernet 5"


InterfaceAlias       : Ethernet 5
InterfaceIndex       : 15
InterfaceDescription : Remote NDIS based Internet Sharing Device #3
NetProfile.Name      : SUHAN
IPv4Address          : 192.168.0.178
IPv6DefaultGateway   :
IPv4DefaultGateway   : 192.168.0.1
DNSServer            : 192.168.0.1



PS C:\Users\ishak> Get-PnpDevice -PresentOnly |
>> Where-Object {
>>     $_.InstanceId -match 'VID_19D2&PID_1557'
>> } |
>> Format-List Status,Class,FriendlyName,InstanceId


Status       : OK
Class        : USB
FriendlyName : USB Composite Device
InstanceId   : USB\VID_19D2&PID_1557\1234567890ABCDEF

Status       : OK
Class        : Net
FriendlyName : Remote NDIS based Internet Sharing Device #3
InstanceId   : USB\VID_19D2&PID_1557&MI_00\6&23391696&0&0000



PS C:\Users\ishak> Get-PnpDevice -PresentOnly |
>> Where-Object {
>>     $_.FriendlyName -match 'ADB|Android'
>> } |
>> Format-Table Status,Class,FriendlyName,InstanceId -AutoSize
PS C:\Users\ishak> adb devices -l
adb : The term 'adb' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the
spelling of the name, or if a path was included, verify that the path is correct and try again.
At line:1 char:1
+ adb devices -l
+ ~~~
    + CategoryInfo          : ObjectNotFound: (adb:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException


Suggestion [3,General]: The command adb was not found, but does exist in the current location. Windows PowerShell does not load commands from the current location by default. If you trust this command, instead type: ".\adb". See "get-help about_Command_Precedence" for more details.
PS C:\Users\ishak> Get-PnpDevice -PresentOnly |
>> Where-Object {
>>     $_.Class -eq 'Ports' -or
>>     $_.FriendlyName -match 'Diagnostic|Modem|Serial|COM'
>> } |
>> Format-Table Status,Class,FriendlyName,InstanceId -AutoSize

Status   Class             FriendlyName                                      InstanceId
------   -----             ------------                                      ----------
OK       System            Microsoft ACPI-Compliant Embedded Controller      ACPI\PNP0C09\1
OK       Battery           Microsoft ACPI-Compliant Control Method Battery   ACPI\PNP0C0A\1
OK       System            Intel(R) Serial IO I2C Host Controller - 9D60     PCI\VEN_8086&DEV_9D60&SUBSYS_83B3103C&R...
OK       System            Composite Bus Enumerator                          ROOT\COMPOSITEBUS\0000
Degraded SoftwareComponent Intel(R) XTU Component Device                     SWD\DRIVERENUM\{D6711452-0DD6-4AEB-A58E...
OK       SoftwareComponent Generic software component                        SWD\DRIVERENUM\OEM_DAL_COMPONENT&4&30BE...
Degraded SoftwareComponent Intel(R) XTU Component Device                     SWD\DRIVERENUM\{D6711452-0DD6-4AEB-A58E...
OK       SoftwareComponent Intel(R) Graphics Command Center                  SWD\DRIVERENUM\IGCC&4&FA60176&0
OK       HIDClass          HID-compliant wireless radio controls             HID\HPQ6001\3&1F700816&0&0000
Degraded SoftwareComponent Intel(R) XTU Component Device                     SWD\DRIVERENUM\{D6711452-0DD6-4AEB-A58E...
Degraded SoftwareComponent Intel(R) XTU Component Device                     SWD\DRIVERENUM\{D6711452-0DD6-4AEB-A58E...
OK       USB               USB Composite Device                              USB\VID_04F2&PID_B5EE\5&1F409D78&0&9
OK       System            PCI Express Root Complex                          ACPI\PNP0A08\0
OK       System            Microsoft ACPI-Compliant System                   ACPI_HAL\PNP0C08\0
OK       System            Intel(R) Serial IO GPIO Host Controller - INT344B ACPI\INT344B\3&11583659&0
OK       System            Microsoft UEFI-Compliant System                   ACPI_HAL\UEFI\0
Degraded SoftwareComponent Intel(R) XTU Component Device                     SWD\DRIVERENUM\{D6711452-0DD6-4AEB-A58E...
OK       Mouse             HID-compliant mouse                               HID\VID_10C4&PID_0005\6&1D057AB9&0&0000
Degraded SoftwareComponent Intel(R) XTU Component Device                     SWD\DRIVERENUM\{D6711452-0DD6-4AEB-A58E...
Degraded SoftwareComponent Intel(R) XTU Component Device                     SWD\DRIVERENUM\{D6711452-0DD6-4AEB-A58E...
OK       System            Intel(R) Serial IO I2C Host Controller - 9D61     PCI\VEN_8086&DEV_9D61&SUBSYS_83B3103C&R...
Degraded SoftwareComponent Intel(R) XTU Component Device                     SWD\DRIVERENUM\{D6711452-0DD6-4AEB-A58E...
OK       USB               USB Composite Device                              USB\VID_19D2&PID_1557\1234567890ABCDEF


PS C:\Users\ishak> [System.IO.Ports.SerialPort]::GetPortNames()
PS C:\Users\ishak> Get-PnpDevice -PresentOnly | Where-Object {$_.FriendlyName -match 'ADB|Android'} | Format-Table Status,Class,FriendlyName,InstanceId -AutoSize
PS C:\Users\ishak> Get-PnpDevice -PresentOnly | Where-Object {$_.Class -eq 'Ports' -or $_.FriendlyName -match 'Diagnostic|Modem|Serial|COM'} | Format-Table Status,Class,FriendlyName,InstanceId -AutoSize

Status   Class             FriendlyName                                      InstanceId
------   -----             ------------                                      ----------
OK       System            Microsoft ACPI-Compliant Embedded Controller      ACPI\PNP0C09\1
OK       Battery           Microsoft ACPI-Compliant Control Method Battery   ACPI\PNP0C0A\1
OK       System            Intel(R) Serial IO I2C Host Controller - 9D60     PCI\VEN_8086&DEV_9D60&SUBSYS_83B3103C&R...
OK       System            Composite Bus Enumerator                          ROOT\COMPOSITEBUS\0000
Degraded SoftwareComponent Intel(R) XTU Component Device                     SWD\DRIVERENUM\{D6711452-0DD6-4AEB-A58E...
OK       SoftwareComponent Generic software component                        SWD\DRIVERENUM\OEM_DAL_COMPONENT&4&30BE...
Degraded SoftwareComponent Intel(R) XTU Component Device                     SWD\DRIVERENUM\{D6711452-0DD6-4AEB-A58E...
OK       SoftwareComponent Intel(R) Graphics Command Center                  SWD\DRIVERENUM\IGCC&4&FA60176&0
OK       HIDClass          HID-compliant wireless radio controls             HID\HPQ6001\3&1F700816&0&0000
Degraded SoftwareComponent Intel(R) XTU Component Device                     SWD\DRIVERENUM\{D6711452-0DD6-4AEB-A58E...
Degraded SoftwareComponent Intel(R) XTU Component Device                     SWD\DRIVERENUM\{D6711452-0DD6-4AEB-A58E...
OK       USB               USB Composite Device                              USB\VID_04F2&PID_B5EE\5&1F409D78&0&9
OK       System            PCI Express Root Complex                          ACPI\PNP0A08\0
OK       System            Microsoft ACPI-Compliant System                   ACPI_HAL\PNP0C08\0
OK       System            Intel(R) Serial IO GPIO Host Controller - INT344B ACPI\INT344B\3&11583659&0
OK       System            Microsoft UEFI-Compliant System                   ACPI_HAL\UEFI\0
Degraded SoftwareComponent Intel(R) XTU Component Device                     SWD\DRIVERENUM\{D6711452-0DD6-4AEB-A58E...
OK       Mouse             HID-compliant mouse                               HID\VID_10C4&PID_0005\6&1D057AB9&0&0000
Degraded SoftwareComponent Intel(R) XTU Component Device                     SWD\DRIVERENUM\{D6711452-0DD6-4AEB-A58E...
Degraded SoftwareComponent Intel(R) XTU Component Device                     SWD\DRIVERENUM\{D6711452-0DD6-4AEB-A58E...
OK       System            Intel(R) Serial IO I2C Host Controller - 9D61     PCI\VEN_8086&DEV_9D61&SUBSYS_83B3103C&R...
Degraded SoftwareComponent Intel(R) XTU Component Device                     SWD\DRIVERENUM\{D6711452-0DD6-4AEB-A58E...
OK       USB               USB Composite Device                              USB\VID_19D2&PID_1557\1234567890ABCDEF


PS C:\Users\ishak> [System.IO.Ports.SerialPort]::GetPortNames()
PS C:\Users\ishak> [System.IO.Ports.SerialPort]::GetPortNames()
PS C:\Users\ishak> pnputil /enum-devices /connected
Microsoft PnP Utility

Instance ID:                INTELAUDIO\FUNC_01&VEN_14F1&DEV_2008&SUBSYS_103C83B3&REV_1001\4&de15d10&0&0001
Device Description:         Conexant ISST Audio
Class Name:                 MEDIA
Class GUID:                 {4d36e96c-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Conexant
Status:                     Started
Driver Name:                oem45.inf

Instance ID:                ACPI\PNP0C02\1
Device Description:         Motherboard resources
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Stopped
Driver Name:                machine.inf

Instance ID:                ACPI\PNP0C02\2
Device Description:         Motherboard resources
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Stopped
Driver Name:                machine.inf

Instance ID:                ACPI\PNP0C02\5
Device Description:         Motherboard resources
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Stopped
Driver Name:                machine.inf

Instance ID:                SW\{96E080C7-143C-11D1-B40F-00A0C9223196}\{3C0D501A-140B-11D1-B40F-00A0C9223196}
Device Description:         Microsoft Streaming Service Proxy
Class Name:                 MEDIA
Class GUID:                 {4d36e96c-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                ksfilter.inf

Instance ID:                ACPI\PNP0C09\1
Device Description:         Microsoft ACPI-Compliant Embedded Controller
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                machine.inf

Instance ID:                ACPI\PNP0C0A\1
Device Description:         Microsoft ACPI-Compliant Control Method Battery
Class Name:                 Battery
Class GUID:                 {72631e54-78a4-11d0-bcf7-00aa00b7b32a}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                cmbatt.inf

Instance ID:                PCI\VEN_8086&DEV_9D21&SUBSYS_83B3103C&REV_21\3&11583659&0&FA
Device Description:         Mobile Intel(R) Processor Family I/O PMC - 9D21
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          INTEL
Status:                     Stopped
Driver Name:                oem68.inf

Instance ID:                ACPI\ThermalZone\CHGZ
Device Description:         ACPI Thermal Zone
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                machine.inf

Instance ID:                ACPI\ACPI000C\2&daba3ff&0
Device Description:         ACPI Processor Aggregator
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                acpipagr.inf

Instance ID:                PCI\VEN_8086&DEV_5914&SUBSYS_83B3103C&REV_08\3&11583659&0&00
Device Description:         Intel(R) Host Bridge/DRAM Registers - 5914
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          INTEL
Status:                     Started
Driver Name:                oem17.inf

Instance ID:                SWD\PRINTENUM\PrintQueues
Device Description:         Root Print Queue
Class Name:                 PrintQueue
Class GUID:                 {1ed2bbf9-11f0-4084-b21f-ad83a8e6dcdc}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                printqueue.inf

Instance ID:                PCI\VEN_8086&DEV_9D60&SUBSYS_83B3103C&REV_21\3&11583659&0&A8
Device Description:         Intel(R) Serial IO I2C Host Controller - 9D60
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Intel Corporation
Status:                     Started
Driver Name:                oem77.inf

Instance ID:                ACPI\ThermalZone\CPUZ
Device Description:         ACPI Thermal Zone
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                machine.inf

Instance ID:                ROOT\volmgr\0000
Device Description:         Volume Manager
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                volmgr.inf

Instance ID:                ACPI\SYNA3070\4&2cb1f961&0
Device Description:         I2C HID Device
Class Name:                 HIDClass
Class GUID:                 {745a17a0-74d3-11d0-b6fe-00a0c90f57da}
Manufacturer Name:          Microsoft
Status:                     Problem
Problem Code:               10 (0x0A) [CM_PROB_FAILED_START]
Problem Status:             0xC000009E
Driver Name:                hidi2c.inf

Instance ID:                PCI\VEN_8086&DEV_9D14&SUBSYS_83B3103C&REV_F1\3&11583659&0&E4
Device Description:         Mobile Intel(R) Processor Family I/O PCI Express Root Port #5 - 9D14
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          INTEL
Status:                     Started
Driver Name:                oem68.inf

Instance ID:                PCI\VEN_8086&DEV_9D71&SUBSYS_83B3103C&REV_21\3&11583659&0&FB
Device Description:         Intel(R) Smart Sound Technology (Intel(R) SST) Audio Controller
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Intel(R) Corporation
Status:                     Started
Driver Name:                oem22.inf

Instance ID:                ACPI\INT3400\2&daba3ff&0
Device Description:         Intel(R) Dynamic Platform and Thermal Framework Manager
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                oem30.inf

Instance ID:                SWD\MSRRAS\MS_PPPOEMINIPORT
Device Description:         WAN Miniport (PPPOE)
Class Name:                 Net
Class GUID:                 {4d36e972-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                netrasa.inf

Instance ID:                SWD\DRIVERENUM\OEM_ICLS_component&4&30be9e7&0
Device Description:         Intel(R) iCLS Client
Class Name:                 SoftwareComponent
Class GUID:                 {5c4c3332-344d-483c-8739-259e934c9cc8}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                oem97.inf

Instance ID:                ROOT\VMBus\0000
Device Description:         Microsoft Hyper-V Virtual Machine Bus Provider
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                wvmbusr.inf

Instance ID:                SWD\MMDEVAPI\{0.0.0.00000000}.{4efb39aa-da82-44d8-92ae-3daeb620cd20}
Device Description:         Speakers (Conexant ISST Audio)
Class Name:                 AudioEndpoint
Class GUID:                 {c166523c-fe0c-4a94-a586-f1a80cfbbf3e}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                audioendpoint.inf

Instance ID:                ROOT\BasicDisplay\0000
Device Description:         Microsoft Basic Display Driver
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard display types)
Status:                     Started
Driver Name:                basicdisplay.inf

Instance ID:                USB\VID_10C4&PID_0005\5&1f409d78&0&2
Device Description:         USB Input Device
Class Name:                 HIDClass
Class GUID:                 {745a17a0-74d3-11d0-b6fe-00a0c90f57da}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                input.inf

Instance ID:                ACPI\INT3403\SEN1
Device Description:         Intel(R) Dynamic Platform and Thermal Framework Generic Participant
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                oem30.inf

Instance ID:                ACPI\INT3403\SEN2
Device Description:         Intel(R) Dynamic Platform and Thermal Framework Generic Participant
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                oem30.inf

Instance ID:                ACPI\HPQ6001\2&daba3ff&0
Device Description:         HP Wireless Button Driver
Class Name:                 HIDClass
Class GUID:                 {745a17a0-74d3-11d0-b6fe-00a0c90f57da}
Manufacturer Name:          HP Inc.
Status:                     Started
Driver Name:                oem85.inf

Instance ID:                ACPI\PNP0C0E\2&daba3ff&0
Device Description:         ACPI Sleep Button
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                machine.inf

Instance ID:                ACPI\PNP0000\4&16f11d76&0
Device Description:         Programmable interrupt controller
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Stopped
Driver Name:                machine.inf

Instance ID:                ROOT\hvservice\0000
Device Description:         Microsoft Hypervisor Service
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                hvservice.inf

Instance ID:                SWD\DRIVERENUM\{9a196a35-8ad1-4819-b54a-c8c7acd7fc20}#HPHotkeySupportUWP.10.0...19041&5&57fd3e9&0
Device Description:         HP LAN/WLAN/WWAN Switching and Hotkey Service
Class Name:                 SoftwareComponent
Class GUID:                 {5c4c3332-344d-483c-8739-259e934c9cc8}
Manufacturer Name:          HP Inc.
Status:                     Started
Driver Name:                oem88.inf

Instance ID:                PCI\VEN_8086&DEV_1903&SUBSYS_83B3103C&REV_08\3&11583659&0&20
Device Description:         Intel(R) Dynamic Platform and Thermal Framework Processor Participant
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                oem36.inf

Instance ID:                SWD\MSRRAS\{5e259276-bc7e-40e3-b93b-8f89b5f3abc0}
Device Description:         Microsoft RRAS Root Enumerator
Class Name:                 SoftwareDevice
Class GUID:                 {62f9c741-b25a-46ce-b54c-9bccce08b6f2}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                c_swdevice.inf

Instance ID:                ACPI\PNP0C14\1
Device Description:         Microsoft Windows Management Interface for ACPI
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                wmiacpi.inf

Instance ID:                ACPI\PNP0C14\2
Device Description:         Microsoft Windows Management Interface for ACPI
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                wmiacpi.inf

Instance ID:                USB\VID_8087&PID_0A2B\5&1f409d78&0&7
Device Description:         Intel(R) Wireless Bluetooth(R)
Class Name:                 Bluetooth
Class GUID:                 {e0cbf06c-cd8b-4647-bb8a-263b43f0f974}
Manufacturer Name:          Intel Corporation
Status:                     Started
Driver Name:                oem31.inf
Extension Driver Names:     oem64.inf

Instance ID:                ACPI\INT3F0D\4&16f11d76&0
Device Description:         Motherboard resources
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Stopped
Driver Name:                machine.inf

Instance ID:                ACPI\USBC000\1
Device Description:         UCM-UCSI ACPI Device
Class Name:                 UCM
Class GUID:                 {e6f1aa1c-7f3b-4473-b2e8-c97d8ac71d53}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                ucmucsiacpiclient.inf

Instance ID:                SWD\MSRRAS\MS_PPTPMINIPORT
Device Description:         WAN Miniport (PPTP)
Class Name:                 Net
Class GUID:                 {4d36e972-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                netrasa.inf

Instance ID:                ROOT\VID\0000
Device Description:         Microsoft Hyper-V Virtualization Infrastructure Driver
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                wvid.inf

Instance ID:                USB\VID_04F2&PID_B5EE&MI_00\6&6e7f1e7&0&0000
Device Description:         HP HD Camera
Class Name:                 Camera
Class GUID:                 {ca3e7ab9-b4c3-4ae6-8251-579ef933890f}
Manufacturer Name:          Chicony
Status:                     Started
Driver Name:                oem42.inf

Instance ID:                ACPI\PNP0103\0
Device Description:         High precision event timer
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Stopped
Driver Name:                machine.inf

Instance ID:                SWD\MSRRAS\MS_AGILEVPNMINIPORT
Device Description:         WAN Miniport (IKEv2)
Class Name:                 Net
Class GUID:                 {4d36e972-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                netavpna.inf

Instance ID:                ROOT\CompositeBus\0000
Device Description:         Composite Bus Enumerator
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                compositebus.inf

Instance ID:                ROOT\vdrvroot\0000
Device Description:         Microsoft Virtual Drive Enumerator
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                vdrvroot.inf

Instance ID:                SWD\MMDEVAPI\{0.0.1.00000000}.{7203c9bc-62cb-4d94-80c2-cd995f0b899f}
Device Description:         Microphone (Conexant ISST Audio)
Class Name:                 AudioEndpoint
Class GUID:                 {c166523c-fe0c-4a94-a586-f1a80cfbbf3e}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                audioendpoint.inf

Instance ID:                UEFI\RES_{854bef26-b844-4f03-8e09-b64b4b1ee21e}\0
Device Description:         HP Q78 System Firmware
Class Name:                 Firmware
Class GUID:                 {f2e7dd72-6468-4e36-b6f1-6488f42c1b52}
Manufacturer Name:          HP Inc.
Status:                     Started
Driver Name:                oem94.inf

Instance ID:                SWD\RADIO\{1F9BF40E-CC3F-4D65-9431-60E844ED784F}
Device Description:         Wi-Fi
Class Name:                 SoftwareDevice
Class GUID:                 {62f9c741-b25a-46ce-b54c-9bccce08b6f2}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                c_swdevice.inf

Instance ID:                SWD\DRIVERENUM\{d6711452-0dd6-4aeb-a58e-803dba73bf69}#XTUComponent&3&47973d1&0
Device Description:         Intel(R) XTU Component Device
Class Name:                 SoftwareComponent
Class GUID:                 {5c4c3332-344d-483c-8739-259e934c9cc8}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                oem84.inf

Instance ID:                ACPI\ACPI0003\2&daba3ff&0
Device Description:         Microsoft AC Adapter
Class Name:                 Battery
Class GUID:                 {72631e54-78a4-11d0-bcf7-00aa00b7b32a}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                cmbatt.inf

Instance ID:                ROOT\spaceport\0000
Device Description:         Microsoft Storage Spaces Controller
Class Name:                 SCSIAdapter
Class GUID:                 {4d36e97b-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                spaceport.inf

Instance ID:                PCI\VEN_8086&DEV_9D2F&SUBSYS_83B3103C&REV_21\3&11583659&0&A0
Device Description:         Intel(R) USB 3.0 eXtensible Host Controller - 1.0 (Microsoft)
Class Name:                 USB
Class GUID:                 {36fc9e60-c465-11cf-8056-444553540000}
Manufacturer Name:          Generic USB xHCI Host Controller
Status:                     Started
Driver Name:                usbxhci.inf

Instance ID:                SWD\DRIVERENUM\OEM_DAL_component&4&30be9e7&0
Device Description:         Generic software component
Class Name:                 SoftwareComponent
Class GUID:                 {5c4c3332-344d-483c-8739-259e934c9cc8}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                c_swcomponent.inf

Instance ID:                ROOT\KDNIC\0000
Device Description:         Microsoft Kernel Debug Network Adapter
Class Name:                 Net
Class GUID:                 {4d36e972-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                kdnic.inf

Instance ID:                SCSI\Disk&Ven_NVMe&Prod_SK_hynix_BC501_H\5&10b9aae2&0&000000
Device Description:         SK hynix BC501 HFM256GDJTNG-8310A
Class Name:                 DiskDrive
Class GUID:                 {4d36e967-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard disk drives)
Status:                     Started
Driver Name:                disk.inf

Instance ID:                STORAGE\Volume\{7575e814-7cdf-11f1-b31e-806e6f6e6963}#0000000000100000
Device Description:         Volume
Class Name:                 Volume
Class GUID:                 {71a27cdd-812a-11d0-bec7-08002be2092f}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                volume.inf

Instance ID:                PCI\VEN_8086&DEV_9D23&SUBSYS_83B3103C&REV_21\3&11583659&0&FC
Device Description:         Mobile Intel(R) Processor Family I/O SMBUS - 9D23
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          INTEL
Status:                     Stopped
Driver Name:                oem68.inf

Instance ID:                SWD\DRIVERENUM\OEM_WMI_component&4&30be9e7&0
Device Description:         Intel(R) Management Engine WMI Provider
Class Name:                 SoftwareComponent
Class GUID:                 {5c4c3332-344d-483c-8739-259e934c9cc8}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                oem96.inf

Instance ID:                SWD\DRIVERENUM\{d6711452-0dd6-4aeb-a58e-803dba73bf69}#XTUComponent&3&33b39e1e&0
Device Description:         Intel(R) XTU Component Device
Class Name:                 SoftwareComponent
Class GUID:                 {5c4c3332-344d-483c-8739-259e934c9cc8}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                oem84.inf

Instance ID:                ACPI\PNP0C0D\2&daba3ff&0
Device Description:         ACPI Lid
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                machine.inf

Instance ID:                ACPI\PNP0C02\PCHRESV
Device Description:         Motherboard resources
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Stopped
Driver Name:                machine.inf

Instance ID:                ROOT\VKRNLINTVSP\0000
Device Description:         Microsoft Hyper-V NT Kernel Integration VSP
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                wvkrnlintvsp.inf

Instance ID:                SWD\DRIVERENUM\{46011826-b9ac-47cb-ba6c-1a53afd571af}#HPServicesScanSoftware&3&6a84d0&0
Device Description:         HP Services Scan
Class Name:                 SoftwareComponent
Class GUID:                 {5c4c3332-344d-483c-8739-259e934c9cc8}
Manufacturer Name:          HP
Status:                     Started
Driver Name:                oem50.inf

Instance ID:                SWD\DRIVERENUM\IGCC&4&fa60176&0
Device Description:         Intel(R) Graphics Command Center
Class Name:                 SoftwareComponent
Class GUID:                 {5c4c3332-344d-483c-8739-259e934c9cc8}
Manufacturer Name:          Intel Corporation
Status:                     Started
Driver Name:                oem57.inf

Instance ID:                PCI\VEN_1C5C&DEV_1327&SUBSYS_00001C5C&REV_00\4&1915b19b&0&00E8
Device Description:         Standard NVM Express Controller
Class Name:                 SCSIAdapter
Class GUID:                 {4d36e97b-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Standard NVM Express Controller
Status:                     Started
Driver Name:                stornvme.inf

Instance ID:                HID\HPQ6001\3&1f700816&0&0000
Device Description:         HID-compliant wireless radio controls
Class Name:                 HIDClass
Class GUID:                 {745a17a0-74d3-11d0-b6fe-00a0c90f57da}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                input.inf

Instance ID:                SWD\DRIVERENUM\{dbf3223d-8a64-4dae-903e-e2fad8d45bc8}#HPAnalyticsSoftware&3&6a84d0&0
Device Description:         HP Device Health Service
Class Name:                 SoftwareComponent
Class GUID:                 {5c4c3332-344d-483c-8739-259e934c9cc8}
Manufacturer Name:          HP
Status:                     Started
Driver Name:                oem11.inf

Instance ID:                ACPI\ThermalZone\PCHZ
Device Description:         ACPI Thermal Zone
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                machine.inf

Instance ID:                SWD\DRIVERENUM\{96bedf2c-18cb-4a15-b821-5e95ed0fea61}#VocaEffectPack&1&81e9aab&0
Device Description:         Voice Clarity
Class Name:                 AudioProcessingObject
Class GUID:                 {5989fce8-9cd0-467d-8a6a-5419e31529d4}
Manufacturer Name:          Microsoft Corporation
Status:                     Started
Driver Name:                oem2.inf

Instance ID:                ROOT\UMBUS\0000
Device Description:         UMBus Root Bus Enumerator
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                umbus.inf

Instance ID:                SWD\DRIVERENUM\{d6711452-0dd6-4aeb-a58e-803dba73bf69}#XTUComponent&3&ad284bc&0
Device Description:         Intel(R) XTU Component Device
Class Name:                 SoftwareComponent
Class GUID:                 {5c4c3332-344d-483c-8739-259e934c9cc8}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                oem84.inf

Instance ID:                ROOT\CAD\0000
Device Description:         Charge Arbitration Driver
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                ChargeArbitration.inf

Instance ID:                ACPI\ThermalZone\EXTZ
Device Description:         ACPI Thermal Zone
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                machine.inf

Instance ID:                SWD\DRIVERENUM\{d6711452-0dd6-4aeb-a58e-803dba73bf69}#XTUComponent&3&10bbbb9&0
Device Description:         Intel(R) XTU Component Device
Class Name:                 SoftwareComponent
Class GUID:                 {5c4c3332-344d-483c-8739-259e934c9cc8}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                oem84.inf

Instance ID:                SWD\RADIO\{3DB5895D-CC28-44B3-AD3D-6F01A782B8D2}
Device Description:         Microsoft Radio Device Enumeration Bus
Class Name:                 SoftwareDevice
Class GUID:                 {62f9c741-b25a-46ce-b54c-9bccce08b6f2}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                c_swdevice.inf

Instance ID:                PCI\VEN_8086&DEV_9D31&SUBSYS_83B3103C&REV_21\3&11583659&0&A2
Device Description:         Mobile Intel(R) Processor Family I/O Thermal subsystem - 9D31
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          INTEL
Status:                     Stopped
Driver Name:                oem35.inf

Instance ID:                USB\VID_04F2&PID_B5EE\5&1f409d78&0&9
Device Description:         USB Composite Device
Class Name:                 USB
Class GUID:                 {36fc9e60-c465-11cf-8056-444553540000}
Manufacturer Name:          (Standard USB Host Controller)
Status:                     Started
Driver Name:                usb.inf

Instance ID:                SWD\MSDAS\{ce958e9a-424f-4c88-86f4-11314821e75a}
Device Description:         Microsoft Device Association Root Enumerator
Class Name:                 SoftwareDevice
Class GUID:                 {62f9c741-b25a-46ce-b54c-9bccce08b6f2}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                c_swdevice.inf

Instance ID:                PCI\VEN_8086&DEV_9D4E&SUBSYS_83B3103C&REV_21\3&11583659&0&F8
Device Description:         Mobile 7th/8th Generation Intel(R) Processor Family I/O LPC Controller (U with iHDCP2.2 Premium) - 9D4E
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          INTEL
Status:                     Started
Driver Name:                oem68.inf

Instance ID:                ROOT\ACPI_HAL\0000
Device Description:         ACPI x64-based PC
Class Name:                 Computer
Class GUID:                 {4d36e966-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard computers)
Status:                     Started
Driver Name:                hal.inf

Instance ID:                PCI\VEN_8086&DEV_9D13&SUBSYS_83B3103C&REV_F1\3&11583659&0&E0
Device Description:         Mobile Intel(R) Processor Family I/O PCI Express Root Port #4 - 9D13
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          INTEL
Status:                     Started
Driver Name:                oem68.inf

Instance ID:                SWD\MSRRAS\MS_NDISWANBH
Device Description:         WAN Miniport (Network Monitor)
Class Name:                 Net
Class GUID:                 {4d36e972-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                netrasa.inf

Instance ID:                SWD\MSRRAS\MS_NDISWANIP
Device Description:         WAN Miniport (IP)
Class Name:                 Net
Class GUID:                 {4d36e972-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                netrasa.inf

Instance ID:                DISPLAY\XXX0000\4&fa60176&0&UID198195
Device Description:         Generic Monitor (AAA)
Class Name:                 Monitor
Class GUID:                 {4d36e96e-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard monitor types)
Status:                     Started
Driver Name:                monitor.inf

Instance ID:                ACPI\PNP0A08\0
Device Description:         PCI Express Root Complex
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                pci.inf

Instance ID:                ACPI\PNP0C02\1000
Device Description:         Motherboard resources
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Stopped
Driver Name:                machine.inf

Instance ID:                STORAGE\Volume\{7575e814-7cdf-11f1-b31e-806e6f6e6963}#0000000007500000
Device Description:         Volume
Class Name:                 Volume
Class GUID:                 {71a27cdd-812a-11d0-bec7-08002be2092f}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                volume.inf

Instance ID:                ROOT\VPCIVSP\0000
Device Description:         Microsoft Hyper-V PCI Server
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                wvpcivsp.inf

Instance ID:                ACPI\GenuineIntel_-_Intel64_Family_6_Model_142_-_Intel(R)_Core(TM)_i5-8350U_CPU_@_1.70GHz\_1
Device Description:         Intel(R) Core(TM) i5-8350U CPU @ 1.70GHz
Class Name:                 Processor
Class GUID:                 {50127dc3-0f36-415e-a6cc-4cb3be910b65}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                cpu.inf
Extension Driver Names:     oem41.inf

Instance ID:                ACPI\GenuineIntel_-_Intel64_Family_6_Model_142_-_Intel(R)_Core(TM)_i5-8350U_CPU_@_1.70GHz\_2
Device Description:         Intel(R) Core(TM) i5-8350U CPU @ 1.70GHz
Class Name:                 Processor
Class GUID:                 {50127dc3-0f36-415e-a6cc-4cb3be910b65}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                cpu.inf
Extension Driver Names:     oem41.inf

Instance ID:                ACPI\GenuineIntel_-_Intel64_Family_6_Model_142_-_Intel(R)_Core(TM)_i5-8350U_CPU_@_1.70GHz\_3
Device Description:         Intel(R) Core(TM) i5-8350U CPU @ 1.70GHz
Class Name:                 Processor
Class GUID:                 {50127dc3-0f36-415e-a6cc-4cb3be910b65}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                cpu.inf
Extension Driver Names:     oem41.inf

Instance ID:                ACPI\GenuineIntel_-_Intel64_Family_6_Model_142_-_Intel(R)_Core(TM)_i5-8350U_CPU_@_1.70GHz\_4
Device Description:         Intel(R) Core(TM) i5-8350U CPU @ 1.70GHz
Class Name:                 Processor
Class GUID:                 {50127dc3-0f36-415e-a6cc-4cb3be910b65}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                cpu.inf
Extension Driver Names:     oem41.inf

Instance ID:                ACPI\GenuineIntel_-_Intel64_Family_6_Model_142_-_Intel(R)_Core(TM)_i5-8350U_CPU_@_1.70GHz\_5
Device Description:         Intel(R) Core(TM) i5-8350U CPU @ 1.70GHz
Class Name:                 Processor
Class GUID:                 {50127dc3-0f36-415e-a6cc-4cb3be910b65}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                cpu.inf
Extension Driver Names:     oem41.inf

Instance ID:                ACPI\GenuineIntel_-_Intel64_Family_6_Model_142_-_Intel(R)_Core(TM)_i5-8350U_CPU_@_1.70GHz\_6
Device Description:         Intel(R) Core(TM) i5-8350U CPU @ 1.70GHz
Class Name:                 Processor
Class GUID:                 {50127dc3-0f36-415e-a6cc-4cb3be910b65}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                cpu.inf
Extension Driver Names:     oem41.inf

Instance ID:                ACPI\GenuineIntel_-_Intel64_Family_6_Model_142_-_Intel(R)_Core(TM)_i5-8350U_CPU_@_1.70GHz\_7
Device Description:         Intel(R) Core(TM) i5-8350U CPU @ 1.70GHz
Class Name:                 Processor
Class GUID:                 {50127dc3-0f36-415e-a6cc-4cb3be910b65}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                cpu.inf
Extension Driver Names:     oem41.inf

Instance ID:                ACPI\GenuineIntel_-_Intel64_Family_6_Model_142_-_Intel(R)_Core(TM)_i5-8350U_CPU_@_1.70GHz\_8
Device Description:         Intel(R) Core(TM) i5-8350U CPU @ 1.70GHz
Class Name:                 Processor
Class GUID:                 {50127dc3-0f36-415e-a6cc-4cb3be910b65}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                cpu.inf
Extension Driver Names:     oem41.inf

Instance ID:                PCI\VEN_8086&DEV_9D3A&SUBSYS_83B3103C&REV_21\3&11583659&0&B0
Device Description:         Intel(R) Management Engine Interface #1
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                oem37.inf

Instance ID:                ACPI\PNP0C0C\2&daba3ff&0
Device Description:         ACPI Power Button
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                machine.inf

Instance ID:                ACPI_HAL\PNP0C08\0
Device Description:         Microsoft ACPI-Compliant System
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                acpi.inf

Instance ID:                HTREE\ROOT\0
Device Description:         Unknown
Class Name:                 Unknown
Class GUID:                 Unknown
Manufacturer Name:          Unknown
Status:                     Started

Instance ID:                ROOT\BasicRender\0000
Device Description:         Microsoft Basic Render Driver
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                basicrender.inf

Instance ID:                STORAGE\Volume\{7575e814-7cdf-11f1-b31e-806e6f6e6963}#00000018BF200000
Device Description:         Volume
Class Name:                 Volume
Class GUID:                 {71a27cdd-812a-11d0-bec7-08002be2092f}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                volume.inf

Instance ID:                ACPI\INT344B\3&11583659&0
Device Description:         Intel(R) Serial IO GPIO Host Controller - INT344B
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Intel Corporation
Status:                     Started
Driver Name:                oem81.inf

Instance ID:                SWD\MSRRAS\MS_SSTPMINIPORT
Device Description:         WAN Miniport (SSTP)
Class Name:                 Net
Class GUID:                 {4d36e972-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                netsstpa.inf

Instance ID:                ACPI_HAL\UEFI\0
Device Description:         Microsoft UEFI-Compliant System
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                uefi.inf

Instance ID:                ACPI\INT0800\4&16f11d76&0
Device Description:         Legacy device
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Intel
Status:                     Stopped
Driver Name:                machine.inf

Instance ID:                SWD\DRIVERENUM\{d6711452-0dd6-4aeb-a58e-803dba73bf69}#XTUComponent&3&20f9ed79&0
Device Description:         Intel(R) XTU Component Device
Class Name:                 SoftwareComponent
Class GUID:                 {5c4c3332-344d-483c-8739-259e934c9cc8}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                oem84.inf

Instance ID:                HID\VID_10C4&PID_0005\6&1d057ab9&0&0000
Device Description:         HID-compliant mouse
Class Name:                 Mouse
Class GUID:                 {4d36e96f-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                msmouse.inf

Instance ID:                SWD\DRIVERENUM\{d6711452-0dd6-4aeb-a58e-803dba73bf69}#XTUComponent&3&29ecd51b&0
Device Description:         Intel(R) XTU Component Device
Class Name:                 SoftwareComponent
Class GUID:                 {5c4c3332-344d-483c-8739-259e934c9cc8}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                oem84.inf

Instance ID:                {5d624f94-8850-40c3-a3fa-a4fd2080baf3}\vwifimp_wfd\5&2c25a66&1&13
Device Description:         Microsoft Wi-Fi Direct Virtual Adapter
Class Name:                 Net
Class GUID:                 {4d36e972-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                netvwifimp.inf

Instance ID:                {5d624f94-8850-40c3-a3fa-a4fd2080baf3}\vwifimp_wfd\5&2c25a66&1&14
Device Description:         Microsoft Wi-Fi Direct Virtual Adapter #2
Class Name:                 Net
Class GUID:                 {4d36e972-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                netvwifimp.inf

Instance ID:                PCI\VEN_8086&DEV_24FD&SUBSYS_10108086&REV_78\4&12af6fe4&0&00E0
Device Description:         Intel(R) Dual Band Wireless-AC 8265
Class Name:                 Net
Class GUID:                 {4d36e972-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Intel Corporation
Status:                     Started
Driver Name:                oem82.inf

Instance ID:                ACPI\PNP0C02\SARESV
Device Description:         Motherboard resources
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Stopped
Driver Name:                machine.inf

Instance ID:                SWD\MMDEVAPI\{0.0.0.00000000}.{136bff9d-71b1-40a1-aa09-24677cbb7fc4}
Device Description:         AAA (Intel(R) Display Audio)
Class Name:                 AudioEndpoint
Class GUID:                 {c166523c-fe0c-4a94-a586-f1a80cfbbf3e}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                audioendpoint.inf

Instance ID:                ACPI\FixedButton\2&daba3ff&0
Device Description:         ACPI Fixed Feature Button
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                machine.inf

Instance ID:                PCI\VEN_8086&DEV_9D18&SUBSYS_83B3103C&REV_F1\3&11583659&0&E8
Device Description:         Mobile Intel(R) Processor Family I/O PCI Express Root Port #9 - 9D18
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          INTEL
Status:                     Started
Driver Name:                oem68.inf

Instance ID:                PCI\VEN_8086&DEV_5917&SUBSYS_83B3103C&REV_07\3&11583659&0&10
Device Description:         Intel(R) UHD Graphics 620
Class Name:                 Display
Class GUID:                 {4d36e968-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Intel Corporation
Status:                     Started
Driver Name:                oem9.inf
Extension Driver Names:     oem8.inf
                            oem44.inf

Instance ID:                SWD\DRIVERENUM\{d6711452-0dd6-4aeb-a58e-803dba73bf69}#XTUComponent&3&17332476&0
Device Description:         Intel(R) XTU Component Device
Class Name:                 SoftwareComponent
Class GUID:                 {5c4c3332-344d-483c-8739-259e934c9cc8}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                oem84.inf

Instance ID:                {DD8E82AE-334B-49A2-AEAE-AEB0FD5C40DD}\DetectionVerification\5&18d6f7a0&0&0
Device Description:         Detection Verification
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Intel(R) Corporation
Status:                     Started
Driver Name:                oem69.inf

Instance ID:                STORAGE\Volume\{7575e814-7cdf-11f1-b31e-806e6f6e6963}#0000000006500000
Device Description:         Volume
Class Name:                 Volume
Class GUID:                 {71a27cdd-812a-11d0-bec7-08002be2092f}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                volume.inf

Instance ID:                SWD\DRIVERENUM\CUI&4&fa60176&0
Device Description:         Intel(R) Graphics Control Panel
Class Name:                 SoftwareComponent
Class GUID:                 {5c4c3332-344d-483c-8739-259e934c9cc8}
Manufacturer Name:          Intel Corporation
Status:                     Started
Driver Name:                oem93.inf

Instance ID:                PCI\VEN_8086&DEV_9D61&SUBSYS_83B3103C&REV_21\3&11583659&0&A9
Device Description:         Intel(R) Serial IO I2C Host Controller - 9D61
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Intel Corporation
Status:                     Started
Driver Name:                oem77.inf

Instance ID:                SWD\COMPUTER\MFG_HP&FAM_103C_5336AN_HP_EliteBook&PROD_HP_EliteBook_830_G5&SKU_2FZ83AV
Device Description:         HP HP EliteBook 830 G5
Class Name:                 Computer
Class GUID:                 {4d36e966-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                compdev.inf
Extension Driver Names:     VocaEffectPack-Extension.inf

Instance ID:                STORAGE\Volume\{7575e814-7cdf-11f1-b31e-806e6f6e6963}#0000001906400000
Device Description:         Volume
Class Name:                 Volume
Class GUID:                 {71a27cdd-812a-11d0-bec7-08002be2092f}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                volume.inf

Instance ID:                ACPI\PNP0100\4&16f11d76&0
Device Description:         System timer
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Stopped
Driver Name:                machine.inf

Instance ID:                SWD\PRINTENUM\{60CDEF75-85AF-474C-9BC3-9E7DF3CA9AFC}
Device Description:         Fax
Class Name:                 PrintQueue
Class GUID:                 {1ed2bbf9-11f0-4084-b21f-ad83a8e6dcdc}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                printqueue.inf

Instance ID:                ACPI\ThermalZone\LOCZ
Device Description:         ACPI Thermal Zone
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                machine.inf

Instance ID:                ROOT\storvsp\0000
Device Description:         Microsoft Hyper-V Virtual Disk Server
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                wstorvsp.inf

Instance ID:                ACPI\PNP0C02\IoTraps
Device Description:         Motherboard resources
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Stopped
Driver Name:                machine.inf

Instance ID:                INTELAUDIO\DSP_CTLR_DEV_9D71&VEN_8086&DEV_0222&SUBSYS_00000022&REV_0001\4&de15d10&0&0800
Device Description:         Intel(R) Smart Sound Technology (Intel(R) SST) OED
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Intel(R) Corporation
Status:                     Started
Driver Name:                oem54.inf

Instance ID:                ACPI\HPQ8002\4&16f11d76&0
Device Description:         Standard 101/102-Key or Microsoft Natural PS/2 Keyboard for HP Hotkey Support
Class Name:                 Keyboard
Class GUID:                 {4d36e96b-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          HP Inc.
Status:                     Started
Driver Name:                oem60.inf
Extension Driver Names:     oem46.inf

Instance ID:                SWD\DRIVERENUM\{30e20916-51cc-469c-ad2d-eaeee7624b3b}#FusionSoftware.10.0...16299&3&6a84d0&0
Device Description:         HP Application Enabling Services
Class Name:                 SoftwareComponent
Class GUID:                 {5c4c3332-344d-483c-8739-259e934c9cc8}
Manufacturer Name:          HP
Status:                     Started
Driver Name:                oem58.inf

Instance ID:                SWD\MMDEVAPI\MicrosoftGSWavetableSynth
Device Description:         Microsoft GS Wavetable Synth
Class Name:                 SoftwareDevice
Class GUID:                 {62f9c741-b25a-46ce-b54c-9bccce08b6f2}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                c_swdevice.inf

Instance ID:                SWD\ScDeviceEnumBus\0
Device Description:         Smart Card Device Enumeration Bus
Class Name:                 SoftwareDevice
Class GUID:                 {62f9c741-b25a-46ce-b54c-9bccce08b6f2}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                c_swdevice.inf

Instance ID:                SWD\ScDeviceEnumBus\1
Device Description:         Microsoft Passport Container Enumeration Bus
Class Name:                 SoftwareDevice
Class GUID:                 {62f9c741-b25a-46ce-b54c-9bccce08b6f2}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                c_swdevice.inf

Instance ID:                SWD\DRIVERENUM\{d6711452-0dd6-4aeb-a58e-803dba73bf69}#XTUComponent&3&e403cd4&0
Device Description:         Intel(R) XTU Component Device
Class Name:                 SoftwareComponent
Class GUID:                 {5c4c3332-344d-483c-8739-259e934c9cc8}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                oem84.inf

Instance ID:                ACPI\ThermalZone\GFXZ
Device Description:         ACPI Thermal Zone
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                machine.inf

Instance ID:                USB\VID_19D2&PID_1557\1234567890ABCDEF
Device Description:         USB Composite Device
Class Name:                 USB
Class GUID:                 {36fc9e60-c465-11cf-8056-444553540000}
Manufacturer Name:          (Standard USB Host Controller)
Status:                     Started
Driver Name:                usb.inf

Instance ID:                SWD\PRINTENUM\{5DE0C855-D3AB-414C-9BEB-96A1E0AE1522}
Device Description:         Microsoft Print to PDF
Class Name:                 PrintQueue
Class GUID:                 {1ed2bbf9-11f0-4084-b21f-ad83a8e6dcdc}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                printqueue.inf

Instance ID:                ROOT\NdisVirtualBus\0000
Device Description:         NDIS Virtual Network Adapter Enumerator
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                ndisvirtualbus.inf

Instance ID:                ACPI\PNP0C02\SerialIoUart0
Device Description:         Motherboard resources
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Stopped
Driver Name:                machine.inf

Instance ID:                ACPI\SYN3071\4&16f11d76&0
Device Description:         Synaptics Pointing Device
Class Name:                 Mouse
Class GUID:                 {4d36e96f-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Synaptics
Status:                     Started
Driver Name:                oem4.inf

Instance ID:                {8e7bd593-6e6c-4c52-86a6-77175494dd8e}\MsVhdHba\1&3030e83&0&01
Device Description:         Microsoft VHD Loopback Controller
Class Name:                 SCSIAdapter
Class GUID:                 {4d36e97b-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                vhdmp.inf

Instance ID:                USB\ROOT_HUB30\4&2488ff5&0&0
Device Description:         USB Root Hub (USB 3.0)
Class Name:                 USB
Class GUID:                 {36fc9e60-c465-11cf-8056-444553540000}
Manufacturer Name:          (Standard USB HUBs)
Status:                     Started
Driver Name:                usbhub3.inf

Instance ID:                {A87C2E0F-9A46-46b8-8EC4-E33355FBE1F7}\KeyboardFilter\5&57fd3e9&0&01
Device Description:         Keyboard_Filter_01
Class Name:                 Unknown
Class GUID:                 Unknown
Manufacturer Name:          HP Inc.
Status:                     Started
Driver Name:                oem60.inf

Instance ID:                ACPI\ThermalZone\BATZ
Device Description:         ACPI Thermal Zone
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                machine.inf

Instance ID:                ACPI\HPIC000C\2&daba3ff&0
Device Description:         HP Application Driver
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          HP
Status:                     Started
Driver Name:                oem49.inf
Extension Driver Names:     oem10.inf
                            oem12.inf
                            oem61.inf

Instance ID:                ACPI\IFX0763\2&daba3ff&0
Device Description:         Trusted Platform Module 2.0
Class Name:                 SecurityDevices
Class GUID:                 {d94ee5d8-d189-4994-83d2-f68d7d41b0e6}
Manufacturer Name:          (Standard)
Status:                     Started
Driver Name:                tpm.inf

Instance ID:                ROOT\mssmbios\0000
Device Description:         Microsoft System Management BIOS Driver
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                mssmbios.inf

Instance ID:                ACPI\INT340E\2&daba3ff&0
Device Description:         Motherboard resources
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Stopped
Driver Name:                machine.inf

Instance ID:                DISPLAY\AUO472D\4&fa60176&0&UID265988
Device Description:         Integrated Monitor
Class Name:                 Monitor
Class GUID:                 {4d36e96e-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard monitor types)
Status:                     Started
Driver Name:                monitor.inf

Instance ID:                USB\VID_19D2&PID_1557&MI_00\6&23391696&0&0000
Device Description:         Remote NDIS based Internet Sharing Device #3
Class Name:                 Net
Class GUID:                 {4d36e972-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                wceisvista.inf

Instance ID:                PCI\VEN_8086&DEV_15D7&SUBSYS_83B3103C&REV_21\3&11583659&0&FE
Device Description:         Intel(R) Ethernet Connection (4) I219-LM
Class Name:                 Net
Class GUID:                 {4d36e972-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Intel
Status:                     Started
Driver Name:                oem25.inf

Instance ID:                INTELAUDIO\FUNC_01&VEN_8086&DEV_280B&SUBSYS_80860101&REV_1000\4&de15d10&0&0201
Device Description:         Intel(R) Display Audio
Class Name:                 MEDIA
Class GUID:                 {4d36e96c-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Intel(R) Corporation
Status:                     Started
Driver Name:                oem86.inf

Instance ID:                ACPI\PNP0C14\TBFP
Device Description:         Microsoft Windows Management Interface for ACPI
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                wmiacpi.inf

Instance ID:                SWD\RADIO\Bluetooth_7470fd1eba1c
Device Description:         Bluetooth
Class Name:                 SoftwareDevice
Class GUID:                 {62f9c741-b25a-46ce-b54c-9bccce08b6f2}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                c_swdevice.inf

Instance ID:                ROOT\SYSTEM\0000
Device Description:         Plug and Play Software Device Enumerator
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                swenum.inf

Instance ID:                ROOT\RDPBUS\0000
Device Description:         Remote Desktop Device Redirector Bus
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                rdpbus.inf

Instance ID:                ACPI\ThermalZone\HEPZ
Device Description:         ACPI Thermal Zone
Class Name:                 System
Class GUID:                 {4d36e97d-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          (Standard system devices)
Status:                     Started
Driver Name:                machine.inf

Instance ID:                SWD\MSRRAS\MS_NDISWANIPV6
Device Description:         WAN Miniport (IPv6)
Class Name:                 Net
Class GUID:                 {4d36e972-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                netrasa.inf

Instance ID:                SWD\MSRRAS\MS_L2TPMINIPORT
Device Description:         WAN Miniport (L2TP)
Class Name:                 Net
Class GUID:                 {4d36e972-e325-11ce-bfc1-08002be10318}
Manufacturer Name:          Microsoft
Status:                     Started
Driver Name:                netrasa.inf

PS C:\Users\ishak> Get-PnpDevice |
>> Where-Object {
>>     $_.InstanceId -match 'VID_19D2&PID_1557'
>> } |
>> Format-Table Status,Class,FriendlyName,InstanceId -AutoSize

Status Class FriendlyName                                 InstanceId
------ ----- ------------                                 ----------
OK     USB   USB Composite Device                         USB\VID_19D2&PID_1557\1234567890ABCDEF
OK     Net   Remote NDIS based Internet Sharing Device #3 USB\VID_19D2&PID_1557&MI_00\6&23391696&0&0000


PS C:\Users\ishak> Get-CimInstance Win32_SerialPort |
>> Format-Table DeviceID,Name,PNPDeviceID -AutoSize
PS C:\Users\ishak>  $dev = 'USB\VID_19D2&PID_1557\1234567890ABCDEF'
PS C:\Users\ishak>
PS C:\Users\ishak> Get-PnpDeviceProperty -InstanceId $dev |
>> Format-List KeyName,Type,Data


KeyName : DEVPKEY_Device_DeviceDesc
Type    : String
Data    : USB Composite Device

KeyName : DEVPKEY_Device_HardwareIds
Type    : StringList
Data    : {USB\VID_19D2&PID_1557&REV_0101, USB\VID_19D2&PID_1557}

KeyName : DEVPKEY_Device_CompatibleIds
Type    : StringList
Data    : {USB\COMPAT_VID_19D2&DevClass_00&SubClass_00&Prot00, USB\COMPAT_VID_19D2&DevClass_00&SubClass_00,
          USB\COMPAT_VID_19D2&DevClass_00, USB\DevClass_00&SubClass_00&Prot_00...}

KeyName : DEVPKEY_Device_Service
Type    : String
Data    : usbccgp

KeyName : DEVPKEY_Device_Class
Type    : String
Data    : USB

KeyName : DEVPKEY_Device_ClassGuid
Type    : Guid
Data    : {36FC9E60-C465-11CF-8056-444553540000}

KeyName : DEVPKEY_Device_Driver
Type    : String
Data    : {36fc9e60-c465-11cf-8056-444553540000}\0007

KeyName : DEVPKEY_Device_ConfigFlags
Type    : UInt32
Data    : 0

KeyName : DEVPKEY_Device_Manufacturer
Type    : String
Data    : (Standard USB Host Controller)

KeyName : DEVPKEY_Device_LocationInfo
Type    : String
Data    : Port_#0001.Hub_#0001

KeyName : DEVPKEY_Device_PDOName
Type    : String
Data    : \Device\USBPDO-4

KeyName : DEVPKEY_Device_Capabilities
Type    : UInt32
Data    : 148

KeyName : DEVPKEY_Device_BusTypeGuid
Type    : Guid
Data    : {9D7DEBBC-C85D-11D1-9EB4-006008C3A19A}

KeyName : DEVPKEY_Device_LegacyBusType
Type    : UInt32
Data    : 15

KeyName : DEVPKEY_Device_BusNumber
Type    : UInt32
Data    : 0

KeyName : DEVPKEY_Device_EnumeratorName
Type    : String
Data    : USB

KeyName : DEVPKEY_Device_Address
Type    : UInt32
Data    : 1

KeyName : DEVPKEY_Device_PowerData
Type    : Binary
Data    : {56, 0, 0, 0...}

KeyName : DEVPKEY_Device_RemovalPolicy
Type    : UInt32
Data    : 3

KeyName : DEVPKEY_Device_RemovalPolicyDefault
Type    : UInt32
Data    : 3

KeyName : DEVPKEY_Device_InstallState
Type    : UInt32
Data    : 0

KeyName : DEVPKEY_Device_BaseContainerId
Type    : Guid
Data    : {F2E6A928-059B-5B0D-A871-DF512735633E}

KeyName : DEVPKEY_NAME
Type    : String
Data    : USB Composite Device

KeyName : DEVPKEY_Device_InstanceId
Type    : String
Data    : USB\VID_19D2&PID_1557\1234567890ABCDEF

KeyName : DEVPKEY_Device_DevNodeStatus
Type    : UInt32
Data    : 25190410

KeyName : DEVPKEY_Device_ProblemCode
Type    : UInt32
Data    : 0

KeyName : DEVPKEY_Device_BusRelations
Type    : StringList
Data    : {USB\VID_19D2&PID_1557&MI_00\6&23391696&0&0000}

KeyName : DEVPKEY_Device_Parent
Type    : String
Data    : USB\ROOT_HUB30\4&2488ff5&0&0

KeyName : DEVPKEY_Device_Children
Type    : StringList
Data    : {USB\VID_19D2&PID_1557&MI_00\6&23391696&0&0000}

KeyName : DEVPKEY_Device_Siblings
Type    : StringList
Data    : {USB\VID_8087&PID_0A2B\5&1f409d78&0&7, USB\VID_04F2&PID_B5EE\5&1f409d78&0&9,
          USB\VID_10C4&PID_0005\5&1f409d78&0&2}

KeyName : DEVPKEY_Device_SafeRemovalRequired
Type    : Boolean
Data    : False

KeyName : DEVPKEY_Device_ContainerId
Type    : Guid
Data    : {F2E6A928-059B-5B0D-A871-DF512735633E}

KeyName : DEVPKEY_Device_IsPresent
Type    : Boolean
Data    : True

KeyName : DEVPKEY_Device_HasProblem
Type    : Boolean
Data    : False

KeyName : {83DA6326-97A6-4088-9453-A1923F573B29} 15
Type    : Boolean
Data    : True

KeyName : DEVPKEY_Device_IsRebootRequired
Type    : Boolean
Data    : False

KeyName : DEVPKEY_Device_ReportedDeviceIdsHash
Type    : UInt32
Data    : 3832816319

KeyName : DEVPKEY_Device_InLocalMachineContainer
Type    : Boolean
Data    : False

KeyName : DEVPKEY_Device_Stack
Type    : StringList
Data    : {\Driver\usbccgp, \Driver\ACPI, \Driver\USBHUB3}

KeyName : {A8B865DD-2E3D-4094-AD97-E593A70C75D6} 26
Type    : Boolean
Data    : False

KeyName : {3464F7A4-2444-40B1-980A-E0903CB6D912} 10
Type    : UInt32
Data    : 3

KeyName : DEVPKEY_Device_BusReportedDeviceDesc
Type    : String
Data    : DEMO Mobile Boardband

KeyName : DEVPKEY_Device_ConfigurationId
Type    : String
Data    : usb.inf:USB\COMPOSITE,Composite.Dev.NT

KeyName : DEVPKEY_Device_PhysicalDeviceLocation
Type    : Binary
Data    : {2, 198, 114, 0...}

KeyName : DEVPKEY_Device_BiosDeviceName
Type    : String
Data    : \_SB.PCI0.XHC.RHUB.HS01

KeyName : {80497100-8C73-48B9-AAD9-CE387E19C56E} 6
Type    : UInt32
Data    : 0

KeyName : {83DA6326-97A6-4088-9453-A1923F573B29} 3
Type    : String
Data    : usb.inf:392c3d53461eda3a:Composite.Dev.NT:10.0.26100.9818:USB\COMPOSITE

KeyName : {83DA6326-97A6-4088-9453-A1923F573B29} 10
Type    : String
Data    : USB\ROOT_HUB30\4&2488ff5&0&0

KeyName : {83DA6326-97A6-4088-9453-A1923F573B29} 26
Type    : Binary
Data    : {0}

KeyName : DEVPKEY_Device_InstallDate
Type    : FileTime
Data    : 10/4/2026 8:47:24 PM

KeyName : DEVPKEY_Device_FirstInstallDate
Type    : FileTime
Data    : 10/4/2026 8:47:24 PM

KeyName : DEVPKEY_Device_LastArrivalDate
Type    : FileTime
Data    : 11/7/2026 12:12:03 AM

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 2
Type    : String
Data    : {00000000-0000-0000-ffff-ffffffffffff}\0000\4

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 3
Type    : UInt32
Data    : 0

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 4
Type    : UInt32
Data    : 4

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 5
Type    : UInt32
Data    : 0

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 6
Type    : UInt32
Data    : 0

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 13
Type    : UInt32
Data    : 0

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 14
Type    : UInt32
Data    : 4278219462

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 15
Type    : UInt32
Data    : 0

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 16
Type    : Boolean
Data    : True

KeyName : DEVPKEY_Device_LocationPaths
Type    : StringList
Data    : {PCIROOT(0)#PCI(1400)#USBROOT(0)#USB(1), ACPI(_SB_)#ACPI(PCI0)#ACPI(XHC_)#ACPI(RHUB)#ACPI(HS01)}

KeyName : DEVPKEY_Device_DriverDate
Type    : FileTime
Data    : 6/21/2006 6:00:00 AM

KeyName : DEVPKEY_Device_DriverVersion
Type    : String
Data    : 10.0.26100.9818

KeyName : DEVPKEY_Device_DriverDesc
Type    : String
Data    : USB Composite Device

KeyName : DEVPKEY_Device_DriverInfPath
Type    : String
Data    : usb.inf

KeyName : DEVPKEY_Device_DriverInfSection
Type    : String
Data    : Composite.Dev.NT

KeyName : DEVPKEY_Device_MatchingDeviceId
Type    : String
Data    : USB\COMPOSITE

KeyName : DEVPKEY_Device_DriverProvider
Type    : String
Data    : Microsoft

KeyName : DEVPKEY_Device_DriverRank
Type    : UInt32
Data    : 16719878

KeyName : {B5A825B8-3907-40B5-ABCA-8451E2453D5F} 3
Type    : UInt32
Data    : 1



PS C:\Users\ishak> Get-PnpDevice -PresentOnly |
>> Where-Object {
>>     $_.InstanceId -match 'VID_19D2&PID_1557'
>> } |
>> ForEach-Object {
>>     Get-PnpDeviceProperty -InstanceId $_.InstanceId |
>>     Format-List KeyName,Data
>> }


KeyName : DEVPKEY_Device_DeviceDesc
Data    : USB Composite Device

KeyName : DEVPKEY_Device_HardwareIds
Data    : {USB\VID_19D2&PID_1557&REV_0101, USB\VID_19D2&PID_1557}

KeyName : DEVPKEY_Device_CompatibleIds
Data    : {USB\COMPAT_VID_19D2&DevClass_00&SubClass_00&Prot00, USB\COMPAT_VID_19D2&DevClass_00&SubClass_00,
          USB\COMPAT_VID_19D2&DevClass_00, USB\DevClass_00&SubClass_00&Prot_00...}

KeyName : DEVPKEY_Device_Service
Data    : usbccgp

KeyName : DEVPKEY_Device_Class
Data    : USB

KeyName : DEVPKEY_Device_ClassGuid
Data    : {36FC9E60-C465-11CF-8056-444553540000}

KeyName : DEVPKEY_Device_Driver
Data    : {36fc9e60-c465-11cf-8056-444553540000}\0007

KeyName : DEVPKEY_Device_ConfigFlags
Data    : 0

KeyName : DEVPKEY_Device_Manufacturer
Data    : (Standard USB Host Controller)

KeyName : DEVPKEY_Device_LocationInfo
Data    : Port_#0001.Hub_#0001

KeyName : DEVPKEY_Device_PDOName
Data    : \Device\USBPDO-4

KeyName : DEVPKEY_Device_Capabilities
Data    : 148

KeyName : DEVPKEY_Device_BusTypeGuid
Data    : {9D7DEBBC-C85D-11D1-9EB4-006008C3A19A}

KeyName : DEVPKEY_Device_LegacyBusType
Data    : 15

KeyName : DEVPKEY_Device_BusNumber
Data    : 0

KeyName : DEVPKEY_Device_EnumeratorName
Data    : USB

KeyName : DEVPKEY_Device_Address
Data    : 1

KeyName : DEVPKEY_Device_PowerData
Data    : {56, 0, 0, 0...}

KeyName : DEVPKEY_Device_RemovalPolicy
Data    : 3

KeyName : DEVPKEY_Device_RemovalPolicyDefault
Data    : 3

KeyName : DEVPKEY_Device_InstallState
Data    : 0

KeyName : DEVPKEY_Device_BaseContainerId
Data    : {F2E6A928-059B-5B0D-A871-DF512735633E}

KeyName : DEVPKEY_NAME
Data    : USB Composite Device

KeyName : DEVPKEY_Device_InstanceId
Data    : USB\VID_19D2&PID_1557\1234567890ABCDEF

KeyName : DEVPKEY_Device_DevNodeStatus
Data    : 25190410

KeyName : DEVPKEY_Device_ProblemCode
Data    : 0

KeyName : DEVPKEY_Device_BusRelations
Data    : {USB\VID_19D2&PID_1557&MI_00\6&23391696&0&0000}

KeyName : DEVPKEY_Device_Parent
Data    : USB\ROOT_HUB30\4&2488ff5&0&0

KeyName : DEVPKEY_Device_Children
Data    : {USB\VID_19D2&PID_1557&MI_00\6&23391696&0&0000}

KeyName : DEVPKEY_Device_Siblings
Data    : {USB\VID_8087&PID_0A2B\5&1f409d78&0&7, USB\VID_04F2&PID_B5EE\5&1f409d78&0&9,
          USB\VID_10C4&PID_0005\5&1f409d78&0&2}

KeyName : DEVPKEY_Device_SafeRemovalRequired
Data    : False

KeyName : DEVPKEY_Device_ContainerId
Data    : {F2E6A928-059B-5B0D-A871-DF512735633E}

KeyName : DEVPKEY_Device_IsPresent
Data    : True

KeyName : DEVPKEY_Device_HasProblem
Data    : False

KeyName : {83DA6326-97A6-4088-9453-A1923F573B29} 15
Data    : True

KeyName : DEVPKEY_Device_IsRebootRequired
Data    : False

KeyName : DEVPKEY_Device_ReportedDeviceIdsHash
Data    : 3832816319

KeyName : DEVPKEY_Device_InLocalMachineContainer
Data    : False

KeyName : DEVPKEY_Device_Stack
Data    : {\Driver\usbccgp, \Driver\ACPI, \Driver\USBHUB3}

KeyName : {A8B865DD-2E3D-4094-AD97-E593A70C75D6} 26
Data    : False

KeyName : {3464F7A4-2444-40B1-980A-E0903CB6D912} 10
Data    : 3

KeyName : DEVPKEY_Device_BusReportedDeviceDesc
Data    : DEMO Mobile Boardband

KeyName : DEVPKEY_Device_ConfigurationId
Data    : usb.inf:USB\COMPOSITE,Composite.Dev.NT

KeyName : DEVPKEY_Device_PhysicalDeviceLocation
Data    : {2, 198, 114, 0...}

KeyName : DEVPKEY_Device_BiosDeviceName
Data    : \_SB.PCI0.XHC.RHUB.HS01

KeyName : {80497100-8C73-48B9-AAD9-CE387E19C56E} 6
Data    : 0

KeyName : {83DA6326-97A6-4088-9453-A1923F573B29} 3
Data    : usb.inf:392c3d53461eda3a:Composite.Dev.NT:10.0.26100.9818:USB\COMPOSITE

KeyName : {83DA6326-97A6-4088-9453-A1923F573B29} 10
Data    : USB\ROOT_HUB30\4&2488ff5&0&0

KeyName : {83DA6326-97A6-4088-9453-A1923F573B29} 26
Data    : {0}

KeyName : DEVPKEY_Device_InstallDate
Data    : 10/4/2026 8:47:24 PM

KeyName : DEVPKEY_Device_FirstInstallDate
Data    : 10/4/2026 8:47:24 PM

KeyName : DEVPKEY_Device_LastArrivalDate
Data    : 11/7/2026 12:12:03 AM

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 2
Data    : {00000000-0000-0000-ffff-ffffffffffff}\0000\4

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 3
Data    : 0

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 4
Data    : 4

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 5
Data    : 0

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 6
Data    : 0

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 13
Data    : 0

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 14
Data    : 4278219462

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 15
Data    : 0

KeyName : {8DBC9C86-97A9-4BFF-9BC6-BFE95D3E6DAD} 16
Data    : True

KeyName : DEVPKEY_Device_LocationPaths
Data    : {PCIROOT(0)#PCI(1400)#USBROOT(0)#USB(1), ACPI(_SB_)#ACPI(PCI0)#ACPI(XHC_)#ACPI(RHUB)#ACPI(HS01)}

KeyName : DEVPKEY_Device_DriverDate
Data    : 6/21/2006 6:00:00 AM

KeyName : DEVPKEY_Device_DriverVersion
Data    : 10.0.26100.9818

KeyName : DEVPKEY_Device_DriverDesc
Data    : USB Composite Device

KeyName : DEVPKEY_Device_DriverInfPath
Data    : usb.inf

KeyName : DEVPKEY_Device_DriverInfSection
Data    : Composite.Dev.NT

KeyName : DEVPKEY_Device_MatchingDeviceId
Data    : USB\COMPOSITE

KeyName : DEVPKEY_Device_DriverProvider
Data    : Microsoft

KeyName : DEVPKEY_Device_DriverRank
Data    : 16719878

KeyName : {B5A825B8-3907-40B5-ABCA-8451E2453D5F} 3
Data    : 1





KeyName : DEVPKEY_Device_DeviceDesc
Data    : Remote NDIS based Internet Sharing Device

KeyName : DEVPKEY_Device_HardwareIds
Data    : {USB\VID_19D2&PID_1557&REV_0101&MI_00, USB\VID_19D2&PID_1557&MI_00}

KeyName : DEVPKEY_Device_CompatibleIds
Data    : {USB\COMPAT_VID_19d2&Class_e0&SubClass_01&Prot_03, USB\COMPAT_VID_19d2&Class_e0&SubClass_01,
          USB\COMPAT_VID_19d2&Class_e0, USB\Class_e0&SubClass_01&Prot_03...}

KeyName : DEVPKEY_Device_Service
Data    : usbrndis6

KeyName : DEVPKEY_Device_Class
Data    : Net

KeyName : DEVPKEY_Device_ClassGuid
Data    : {4D36E972-E325-11CE-BFC1-08002BE10318}

KeyName : DEVPKEY_Device_Driver
Data    : {4d36e972-e325-11ce-bfc1-08002be10318}\0017

KeyName : DEVPKEY_Device_ConfigFlags
Data    : 0

KeyName : DEVPKEY_Device_Manufacturer
Data    : Microsoft

KeyName : DEVPKEY_Device_FriendlyName
Data    : Remote NDIS based Internet Sharing Device #3

KeyName : DEVPKEY_Device_LocationInfo
Data    : 0000.0014.0000.001.000.000.000.000.000

KeyName : DEVPKEY_Device_PDOName
Data    : \Device\000000c5

KeyName : DEVPKEY_Device_Capabilities
Data    : 128

KeyName : DEVPKEY_Device_BusTypeGuid
Data    : {9D7DEBBC-C85D-11D1-9EB4-006008C3A19A}

KeyName : DEVPKEY_Device_LegacyBusType
Data    : 15

KeyName : DEVPKEY_Device_BusNumber
Data    : 0

KeyName : DEVPKEY_Device_EnumeratorName
Data    : USB

KeyName : DEVPKEY_Device_Address
Data    : 1

KeyName : DEVPKEY_Device_PowerData
Data    : {56, 0, 0, 0...}

KeyName : DEVPKEY_Device_RemovalPolicy
Data    : 3

KeyName : DEVPKEY_Device_RemovalPolicyDefault
Data    : 3

KeyName : DEVPKEY_Device_InstallState
Data    : 0

KeyName : DEVPKEY_Device_BaseContainerId
Data    : {F2E6A928-059B-5B0D-A871-DF512735633E}

KeyName : DEVPKEY_NAME
Data    : Remote NDIS based Internet Sharing Device #3

KeyName : DEVPKEY_Device_InstanceId
Data    : USB\VID_19D2&PID_1557&MI_00\6&23391696&0&0000

KeyName : DEVPKEY_Device_DevNodeStatus
Data    : 25174026

KeyName : DEVPKEY_Device_ProblemCode
Data    : 0

KeyName : DEVPKEY_Device_Parent
Data    : USB\VID_19D2&PID_1557\1234567890ABCDEF

KeyName : DEVPKEY_Device_SafeRemovalRequired
Data    : False

KeyName : DEVPKEY_Device_ContainerId
Data    : {F2E6A928-059B-5B0D-A871-DF512735633E}

KeyName : DEVPKEY_Device_IsPresent
Data    : True

KeyName : DEVPKEY_Device_HasProblem
Data    : False

KeyName : {83DA6326-97A6-4088-9453-A1923F573B29} 15
Data    : True

KeyName : DEVPKEY_Device_IsRebootRequired
Data    : False

KeyName : DEVPKEY_Device_ReportedDeviceIdsHash
Data    : 3538979451

KeyName : DEVPKEY_Device_InLocalMachineContainer
Data    : False

KeyName : DEVPKEY_Device_Stack
Data    : {\Driver\usbrndis6, \Driver\usbccgp}

KeyName : {A8B865DD-2E3D-4094-AD97-E593A70C75D6} 26
Data    : False

KeyName : {3464F7A4-2444-40B1-980A-E0903CB6D912} 10
Data    : 3

KeyName : DEVPKEY_Device_BusReportedDeviceDesc
Data    : RNDIS

KeyName : DEVPKEY_Device_ConfigurationId
Data    : wceisvista.inf:USB\Class_e0&SubClass_01&Prot_03,RNDIS.NT.5.1

KeyName : {80497100-8C73-48B9-AAD9-CE387E19C56E} 6
Data    : 0

KeyName : {83DA6326-97A6-4088-9453-A1923F573B29} 3
Data    : wceisvista.inf:00a2c5fc8697c22f:RNDIS.NT.5.1:10.0.26100.1:USB\Class_e0&SubClass_01&Prot_03

KeyName : {83DA6326-97A6-4088-9453-A1923F573B29} 10
Data    : USB\VID_19D2&PID_1557\1234567890ABCDEF

KeyName : DEVPKEY_Device_InstallDate
Data    : 10/4/2026 8:47:24 PM

KeyName : DEVPKEY_Device_FirstInstallDate
Data    : 10/4/2026 8:47:24 PM

KeyName : DEVPKEY_Device_LastArrivalDate
Data    : 11/7/2026 12:12:03 AM

KeyName : DEVPKEY_Device_LocationPaths
Data    : {PCIROOT(0)#PCI(1400)#USBROOT(0)#USB(1)#USBMI(0), PCIROOT(0)#PCI(1400)#USBROOT(0)#USB(1)#USB(1),
          ACPI(_SB_)#ACPI(PCI0)#ACPI(XHC_)#ACPI(RHUB)#ACPI(HS01)#USBMI(0),
          ACPI(_SB_)#ACPI(PCI0)#ACPI(XHC_)#ACPI(RHUB)#ACPI(HS01)#USB(1)}

KeyName : DEVPKEY_Device_DriverDate
Data    : 6/21/2006 6:00:00 AM

KeyName : DEVPKEY_Device_DriverVersion
Data    : 10.0.26100.1

KeyName : DEVPKEY_Device_DriverDesc
Data    : Remote NDIS based Internet Sharing Device

KeyName : DEVPKEY_Device_DriverInfPath
Data    : wceisvista.inf

KeyName : DEVPKEY_Device_DriverInfSection
Data    : RNDIS.NT.5.1

KeyName : DEVPKEY_Device_MatchingDeviceId
Data    : USB\Class_e0&SubClass_01&Prot_03

KeyName : DEVPKEY_Device_DriverProvider
Data    : Microsoft

KeyName : DEVPKEY_Device_DriverRank
Data    : 16719875

KeyName : {A8B865DD-2E3D-4094-AD97-E593A70C75D6} 16
Data    : {netrndis.inf}
