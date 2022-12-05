﻿<div align="center">

## Code to Connect another computer on LAN


</div>

### Description

This article demonstrates how to programmatically create and remove network connections by using Windows API functions. The following example will add a connection to a network share and will disconnect from the same share.
 
### More Info
 
Computer Name

UserName

Password

U can connect programmatically to another computer on LAN

Connection Successfull

Connection Unsuccessfull

1. Create a new Standard EXE project.

2. Add a module to the project.

3. Copy and paste the following Declares and Type into the module: (NOTE: If you add this to a Form module, make all entries Private.)

4. Add two CommandButtons to Form1. These will be Command1 and Command2 by default.

5. Add the following code to Form1, substituting a valid share name for "\\ServerName\ShareName":

Run the project and click on Command1. You will get a Message dialog indicating success or failure. If successful, you should be able to look in Windows Explorer and see the new connection (unless you left lpLocalName undefined, in which case the connection does not show in Explorer). Click Command2 and go to Explorer, where you should see that the connection has been removed.


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Amitya](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/amitya.md)
**Level**          |Advanced
**User Rating**    |4.8 (24 globes from 5 users)
**Compatibility**  |VB 6\.0
**Category**       |[Internet/ HTML](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/internet-html__1-34.md)
**World**          |[Visual Basic](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/visual-basic.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/amitya-code-to-connect-another-computer-on-lan__1-66898/archive/master.zip)

### API Declarations

```
Declare Function WNetAddConnection2 Lib "mpr.dll" Alias _
   "WNetAddConnection2A" (lpNetResource As NETRESOURCE, _
   ByVal lpPassword As String, ByVal lpUserName As String, _
   ByVal dwFlags As Long) As Long
   Declare Function WNetCancelConnection2 Lib "mpr.dll" Alias _
   "WNetCancelConnection2A" (ByVal lpName As String, _
   ByVal dwFlags As Long, ByVal fForce As Long) As Long
   Type NETRESOURCE
    dwScope As Long
    dwType As Long
    dwDisplayType As Long
    dwUsage As Long
    lpLocalName As String
    lpRemoteName As String
    lpComment As String
    lpProvider As String
   End Type
   Public Const NO_ERROR = 0
   Public Const CONNECT_UPDATE_PROFILE = &amp;H1
   ' The following includes all the constants defined for NETRESOURCE,
   ' not just the ones used in this example.
   Public Const RESOURCETYPE_DISK = &amp;H1
   Public Const RESOURCETYPE_PRINT = &amp;H2
   Public Const RESOURCETYPE_ANY = &amp;H0
   Public Const RESOURCE_CONNECTED = &amp;H1
   Public Const RESOURCE_REMEMBERED = &amp;H3
   Public Const RESOURCE_GLOBALNET = &amp;H2
   Public Const RESOURCEDISPLAYTYPE_DOMAIN = &amp;H1
   Public Const RESOURCEDISPLAYTYPE_GENERIC = &amp;H0
   Public Const RESOURCEDISPLAYTYPE_SERVER = &amp;H2
   Public Const RESOURCEDISPLAYTYPE_SHARE = &amp;H3
   Public Const RESOURCEUSAGE_CONNECTABLE = &amp;H1
   Public Const RESOURCEUSAGE_CONTAINER = &amp;H2
   ' Error Constants:
   Public Const ERROR_ACCESS_DENIED = 5&amp;
   Public Const ERROR_ALREADY_ASSIGNED = 85&amp;
   Public Const ERROR_BAD_DEV_TYPE = 66&amp;
   Public Const ERROR_BAD_DEVICE = 1200&amp;
   Public Const ERROR_BAD_NET_NAME = 67&amp;
   Public Const ERROR_BAD_PROFILE = 1206&amp;
   Public Const ERROR_BAD_PROVIDER = 1204&amp;
   Public Const ERROR_BUSY = 170&amp;
   Public Const ERROR_CANCELLED = 1223&amp;
   Public Const ERROR_CANNOT_OPEN_PROFILE = 1205&amp;
   Public Const ERROR_DEVICE_ALREADY_REMEMBERED = 1202&amp;
   Public Const ERROR_EXTENDED_ERROR = 1208&amp;
   Public Const ERROR_INVALID_PASSWORD = 86&amp;
   Public Const ERROR_NO_NET_OR_BAD_PATH = 1203&amp;
```


### Source Code

```
Option Explicit
   Private Sub Command1_Click()
   Dim NetR As NETRESOURCE
   Dim ErrInfo As Long
   Dim MyPass As String, MyUser As String
   NetR.dwScope = RESOURCE_GLOBALNET
   NetR.dwType = RESOURCETYPE_DISK
   NetR.dwDisplayType = RESOURCEDISPLAYTYPE_SHARE
   NetR.dwUsage = RESOURCEUSAGE_CONNECTABLE
   NetR.lpLocalName = "X:" ' If undefined, Connect with no device
   NetR.lpRemoteName = "\\ServerName\ShareName"  ' Your valid share
   'NetR.lpComment = "Optional Comment"
   'NetR.lpProvider =  ' Leave this undefined
   ' If the MyPass and MyUser arguments are null (use vbNullString), the
   ' user context for the process provides the default user name.
   ErrInfo = WNetAddConnection2(NetR, MyPass, MyUser, _
   CONNECT_UPDATE_PROFILE)
   If ErrInfo = NO_ERROR Then
    MsgBox "Net Connection Successful!", vbInformation, _
    "Share Connected"
   Else
    MsgBox "ERROR: " & ErrInfo & " - Net Connection Failed!", _
    vbExclamation, "Share not Connected"
   End If
   End Sub
   Private Sub Command2_Click()
   Dim ErrInfo As Long
   Dim strLocalName As String
   ' You may specify either the lpRemoteName or lpLocalName
   'strLocalName = "\\ServerName\ShareName"
   strLocalName = "X:"
   ErrInfo = WNetCancelConnection2(strLocalName, _
   CONNECT_UPDATE_PROFILE, False)
   If ErrInfo = NO_ERROR Then
    MsgBox "Net Disconnection Successful!", vbInformation, _
    "Share Disconnected"
   Else
    MsgBox "ERROR: " & ErrInfo & " - Net Disconnection Failed!", _
    vbExclamation, "Share not Disconnected"
   End If
   End Sub
```

