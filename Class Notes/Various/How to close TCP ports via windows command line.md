The command `netstat -ano` displays all TCP ports used. More specifically, the command `netstat -ano | find /i "80"`, shows whatever ports with 80 is used.

Then the PID will appear at the rigth which you can kill with taskkill. Therefore, running the command `taskkill /PID 11704 /F`

![Command View to show how to shut down a TCP port opened](resources/images/ScreenshotPID.png)