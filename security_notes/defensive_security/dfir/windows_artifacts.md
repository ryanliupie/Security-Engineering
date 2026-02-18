## Windows Artifacts 

There are many programs on Microsoft that provide a wealth of information to forensic analysts. These programs were not put by Microsoft to spy on you rather they are needed for other tasks. For instance, there are programs that help launch windows applications or programs that help with backwards compatibility. 

An important thing to grasp is these artifacts often change when there are newer operating system releases. 
<hr>

### Windows Registry

This is a <b>central hierarchical database</b> used to store information that is necessary to configure the system for applications, hardware devices, or users. 

It holds configuration information that Windows actively references to operate the OS. Windows reads this registry constantly so it does not have to "guess" how everything should work. For example, it can include user settings (HKCU), System-wide settings (HKLM), what programs are installed, what file types open with what, hardware/driver configuration, startup/persistence settings (Run/RunOnce). 
<hr> 

### What are Registry Hives?  

A <b>Registry Hive</b> is a top-level folder that contain crucial configuration data, user profiles, and system settings. They form the backbone of the Windows Registry database. Everything under them are <b>Subkeys</b>, and under the Subkeys, there are <b>Values</b> that contain the actual data. There are 5 main registry hives that include: 

<img src="../../../images/hives.png" width="250px" alt="windows_hives">


These can be found in the <b>Registry Editor</b> or the <b>Regedit</b> application in Windows. 


- ### 1. HKEY_CURRENT_USER (HKCU)

    This registry stores settings specific to the user currently logged in. If you open CMD and type in the following: `dir/ah`, you would see a hive file called `NTUSER.DAT`. This file stores most of the user's registry data and so when the user logs in, Windows loads this file and maps it into the registry as that user's hive. Some information includes the users folders, screen colours, and control panel settings. 
    
    During an incident, looking at this file can give us information on a couple of actions that user performed such as: 

    - recently opened files 
    - commands used 
    - application usage 

    Usually when we talk about <b>HKEY_CURRENT_USER (HKCU)</b>, we are talking about the analysis of a <b>live system</b>. 
    
    When we talk about <b>NTUSER.DAT</b>, we are talking about parsing a file on a <b>dead system</b>. A dead system means the machine is powered off and you are analyzing a disk image/copied filed offline. In this case, it would be NTUSER.DAT. 

    



- ### 2. HKEY_LOCAL_MACHINE (HKLM)

    This registry stores system related/configuration settings for a particular computer which affects all the users. 

