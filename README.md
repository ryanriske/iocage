iocage
======

**FreeBSD jail manager**

iocage is a zero dependency drop in jail/container manager amalgamating some
of the best features and technologies FreeBSD operating system has to offer.
It is geared for ease of use with a simple and easy to understand command syntax.

Iocage is in the FreeBSD ports tree.
To install simply run: `pkg install iocage`

- **[DOCUMENTATION](http://iocage.readthedocs.org/en/latest/index.html)**

**FEATURES:**
- rapid thin provisioning (within seconds!)
- templating
- automatic package installation
- ease of use (also supports shortened UUIDs)
- zero configuration files
- virtual networking stacks (vnet)
- shared IP based jails (non vnet)
- fully writable clones
- resource limits (CPU, MEMORY, etc.)
- filesystem quotas and reservations
- ZFS jailing inside jails
- transparent snapshot management
- binary updates
- differential jail packaging
- export and import
- and many more!

**USAGE:**
-  iocage fetch [release=RELEASE | ftphost=ftp.hostname.org] 
-  iocage create [-c|-e] [release=RELEASE] [pkglist=file] [property=value]
-  iocage clone UUID | UUID@snapshot [property=value]
-  iocage destroy UUID
-  iocage list [-t]
-  iocage start UUID
-  iocage stop UUID
-  iocage restart UUID
-  iocage rcboot
-  iocage rcshutdown
-  iocage console UUID
-  iocage chroot UUID
-  iocage df
-  iocage get [property | all ] UUID
-  iocage set property=value UUID
-  iocage cap UUID
-  iocage limts [UUID]
-  iocage uncap UUID
-  iocage inuse UUID
-  iocage snapshot UUID | UUID@snapshotname
-  iocage snaplist UUID
-  iocage snapremove UUID@snapshotname
-  iocage rollback UUID@snapshotname
-  iocage promote UUID
-  iocage runtime UUID
-  iocage update UUID
-  iocage record start|stop UUID
-  iocage package UUID
-  iocage export UUID
-  iocage import UUID [property=value]
-  iocage defaults
-  iocage version | --version
-  iocage help

**REQUIREMENTS**
- FreeBSD 10.0-RELEASE amd64
- ZFS file system
- Kernel compiled with:

        # This is optional and only needed if you need VNET and resource
        # limits

        options         VIMAGE # VNET/Vimage support
        options         RACCT  # Resource containers
        options         RCTL   # same as above

**OTHER CONSIDERATIONS**
- for resource limiting please read rctl(8)
- for the explanations on jail properties read jail(8)
- create bridge0 and bridge1 interfaces 

**QUICK HOWTO**
- 1. add bridge configuration to `/etc/rc.conf` on the host node
   `cloned_interfaces="bridge0 bridge1"`
- 2. run `iocage fetch` - this will fetch the current release and prepare the
   base jail environment. Optionally release can be overridden by issuing 
   `iocage fetch release=9.2-RELEASE` if you intend to run other releases.
- 3. execute `iocage create` - this will set up a jail from scratch. If needed
   The -c option will create a thin jail (ZFS clone) example: `iocage create -c`
   the `tag=any_name` can be used to tag a jail at creation.
- 4. issue `iocage list`
- 5. start the jail with `iocage start uuid`
- 6. drop into jail with `iocage console uuid`

**OPTIONAL**
- turn on resource caps with `iocage set rlimits=on uuid`
- reload limits on-the-fly `iocage cap uuid`
- list resource usage `iocage inuse uuid`
- release limits `iocage uncap uuid`

**HINTS**
- Use iocage set/get to modify properties
- To understand what most properties do read jail(8)
- Consider adding the following to `/etc/sysctl.conf` on the host:

        net.inet.ip.forwarding=1       # Enable IP forwarding between interfaces
        net.link.bridge.pfil_onlyip=0  # Only pass IP packets when pfil is enabled
        net.link.bridge.pfil_bridge=0  # Packet filter on the bridge interface
        net.link.bridge.pfil_member=0  # Packet filter on the member interface
