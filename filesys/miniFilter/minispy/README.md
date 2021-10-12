---
page_type: sample
description: "A tool to monitor and log any I/O and transaction activity that occurs in the system."
languages:
- cpp
products:
- windows
- windows-wdk
---

# Minispy File System Minifilter Driver

The Minispy sample is a tool to monitor and log any I/O and transaction activity that occurs in the system. Minispy is implemented as a minifilter.

## Universal Windows Driver Compliant

This sample builds a Universal Windows Driver. It uses only APIs and DDIs that are included in OneCoreUAP.

## Design and Operation

Minispy consists of both user-mode and kernel-mode components. The kernel-mode component registers callback functions that correspond to various I/O and transaction operations with the filter manager. These callback functions help Minispy record any I/O and transaction activity occurring in the system. When a user can request the recorded information, the recorded information is passed to the user-mode component, which can either output it on screen or log it to a file on disk.

To observe I/O activity on a device, you must explicitly attach Minispy to that device by using the Minispy user-mode component. Similarly, you can request Minispy to stop logging data for a particular device.

For more information on file system minifilter design, start with the [File System Minifilter Drivers](https://docs.microsoft.com/windows-hardware/drivers/ifs/file-system-minifilter-drivers) section in the Installable File Systems Design Guide.


# Compile and Install
## Setup and Deploy
This project is divided into two parts:
1. Kernel Mode Service
2. User-Land Application

For the begining, Read the following documentation to become familiar with file system mini filter driver development and it's concept: [Filter Manager Concepts
]([https://link](https://docs.microsoft.com/en-us/windows-hardware/drivers/ifs/filter-manager-concepts))

After cloning the repository, install these software, tools, and SDK based on your development environment:

### Preparing the Development Environment
1. Install VS 2019 Community or Enterprise. If you are student of Florida International University, Then you may be eligible for claiming your free license thoughought university credential [Microsoft Azure - CS@FIU](https://azureforeducation.microsoft.com/devtools)
2. Since testing kernel module development makes you prone to to see a famous BluScreen, You need to have a separate windows 10 as guest OS inside of virtual machine VB. So, Grab your windows 10 education version [Download](https://azureforeducation.microsoft.com/devtools)
3. After cloning your repository you have two option for compiling this project.
4. Install VS 2015-2019 Reditribution tools for based on your current architecture (x86 or x64)
5. Install WDK for further auto-process and swift deplyment

### Compiling the project and retreiving output
6. - Develop your INF file which meet [DCH](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/dch-principles-best-practices) standard requirement. Fortunately, for begining you can bypass by a simple trick
   - Keep using current version of INF file which is not recommended
7. Compile the project and applying target features
   - If you machine is 64bit -> Select Release & x64
   - If you machine is 32bit -> Select Release & Win32
8. Do not forget to include Driver package location for signing. Do not know how? Just follow these steps:
   1. *Right-click* on your kernel minispy project then select *Properties*
   2. Go to *Configuration Properties* then select *ApiValidator*
   3. Paste your Release/Debug address which is containing your project output (somewhere containing minispy.sys)
   4. Done.
9.  Copy following pairs to your virtual machine in order to deploy your first test
   - minispy.exe
   - minispy.sys (signed version)
   - mnispy.inf
11. Congratulation! you output is ready. Drink a coffee then follow next steps :)

### Running the project files
12. You driver is self-signed by VS. You have two options for runnig this driver
    1.  Due to windows protection against installing unwanted and unknown drivers, you need to pay money in order to receive your certificate for signing this driver. if you have then good for you!
    2.  You do not have money? Do not worry about it at all. You can do this freely if you accept it's dangerous and risk at your responsiblity. You  may disable your windows driver signing check in order to deploy your driver
    3. Run these commands in CMD.exe aith Admin right to disable windows driver signing check and also connect it to your VS for automatic deploy:
       1. > `bcdedit /debug on`
       2. > `bcdedit /dbgsettings net hostip:ANY_IP port:DEBUG_PORT > DBGKey.txt`
       3. > `bcdedit /set testsigning on`
       4. > `bcdedit /dbgsettings`
    4. After runnin the last command, you will see something like this message:
        > "The operation done successfully"
    5. Done.
13. Restart your guest windows 10 in order to enter new environment for driver testing...
14. To be sure that you COMPLETELY disabled the driver sign checking, You will this message in buttom-right [near your windows clock]
    >Test Mode
15. Right clock on your minispy.inf then click "Install"
16. open your CMD.exe with Admin priviledge then run this command to start this service:
    > sc start minispy
17. You may run minispy.exe now.
18. Done.