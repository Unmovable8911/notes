# Introducing `/etc/default/grub`
The path of grub2 configuration file is `/boot/grub/grub.cfg`, but this file should not be edited by hand.  
You should configure grub by editing `/etc/default/grub`, After making changes to the this file, you need to run `sudo update-grub`
to sychronize the changes to grub.

# Configuring grub
Open `/etc/default/grub` with any text editor you prefer, you should see something like this:
```
GRUB_DEFAULT=0                                                                                                                                                                             
#GRUB_TIMEOUT_STYLE=hidden

GRUB_TIMEOUT=10
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
GRUB_CMDLINE_LINUX=""
 ```
 `GRUB_DEFAULT` is the index of the default start up entry, starts from 0.  
 `GRUB_TIMEOUT` is the interval of time grub starts up the the default entry.  
 As for other options, just keep them the default.
