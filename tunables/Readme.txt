Issue with mounting osxfuse via automount or fstab:
https://github.com/osxfuse/osxfuse/issues/171

The fix is to run:
$ sudo sysctl -w osxfuse.tunables.allow_other=1

To make it reboot resistant, copy sysctl.plist to /Library/LaunchDaemons/sysctl.plist
and start it once:
$ sudo launchctl load /Library/LaunchDaemons/sysctl.plist

