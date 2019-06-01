# INSTALL_VERIFICATION_FAILED_ALERT_INFO Error with Office 365 and macOS

While installing Office 365 (16.25.19051201) for a client (macOS High Sierra 10.13.6), Microsoft AutoUpdate (MAU) pops up with an error saying "INSTALL_VERIFICATION_FAILED_ALERT_INFO". Clicking OK would open the browser and it would proceed to download the latest AutoUpdate to install. However, at the end of the install the error comes up again. Repeat until annoyed.

While going through many Q & A pages, I recall running into a folder that I couldn't access without elevated privileges. I don't know macOS internals that well so I didn't have the slightest idea as to the purpose of that directory. 

It turns out that the permissions on `/Library/PrivilegedHelperTools` appeared to be completely borked. Unfortunately, I didn't capture what the state was but it was probably something along the lines of `rwx------`.

Comparing with two devices I had access to, the permissions were the following:

```
# ls -ld /Library/PrivilegedHelperTools
drwxr-xr-t  13 root  wheel  416 Feb 22 18:32 /Library/PrivilegedHelperTools
```

As root, I semi-sorted the permissions with `# chmod 755 /Library/PrivilegedHelperTools` and rebooted. Upon reboot, the permissions were `rwxr-xr-t` and MAU and friends appeared to be happy.
