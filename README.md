# Roll Your Own Router With Shell Scripts
This project allows you to turn commodity PC hardware into a Linux based home
router, while controlling every aspect of the router and it's tooling.  It is
designed to run on el9 (RHEL, CentOS Stream, Rocky, Alma).  The intended
audience is the advanced Linux user who wants extreme control and power over
their router, and would prefer to use SSH and the command line to administer
the router instead of a web interface such as DD-WRT, OpenWRT, etc..

## Advantages
- Infinitely customizable, add additional features and configurations using
  Linux shell scripts, RPM spec files and `/etc` config files
- Automatically install the latest security updates
- Upgradable hardware, for example, you can replace your 1Gb/s NICs with
  10Gb/s NICs
- Potentially much more CPU power than a standard passively cooled MIPS or ARM
  router

## Disadvantages
- Not easy to bootstrap a new router, nor is it possible to write a
  comprehensive guide.  You should be at least an intermediate level Linux user
  who is comfortable reverse engineering the default scripts and configurations
  before trying this, unless you are willing to be very patient and learn many
  new things.  It is advisable to try it first on an old PC before spending
  money to build a new one.
- You will probably need to keep it plugged in to a keyboard, mouse and monitor
  while you are developing your own customizations.  Eventually, you should be
  able to use it as a headless appliance
- May consume more power than a standard router, although anecdotally it
  seems that it consumes little power and generates little heat on an entry
  level Ryzen G series CPU.

# Hardware
- Runs on any CPU architecture supported by el9 (x86\_64, PowerPC, ARM).
- When choosing a motherboard and case form factor, take into account how many
  PCIe expansion cards you will need for NICs, wifi cards and other accessories
- If using as a wireless router, recommended to use a wifi card using a
  Qualcomm Atheros chip, for best compatibility with hostapd access points
- You will need at least one NIC for plugging into your modem, and additional
  NICs for any wired networking that you wish to do

## BIOS Settings
You should, at a minimum, make the following BIOS settings if available:
- Computer always on after power failure -- Ensures that the router will stay
  running at all times, even after a temporary loss of power

# Installing
NOTE: Only do this on the machine that you intend to use as a router, otherwise
it will likely render your networking inoperable, and not easy to repair.

NOTE: When bootstrapping a new router for the first time, it is far easier to
plug it into an existing router operating in a different subnet, and slowly
move all of your devices over to the new router, before eventually plugging
the new router directly into the modem (not applicable to all types of ISPs).

- Clone the git repository and `cd` into the directory.
- Update `files/etc/ryor/config.yaml` to match your hardware specs
- Update all of the config files in `files/etc`, or some can be deleted as
  desired from `tools/rpm.spec`
  - You may also need to modify some scripts such as `create-network.sh`
- Run `tools/el9-deps.sh` to install required dependencies
- Run `make rpm` to build the RPM, which can be installed with
  `sudo dnf install ./$PACKAGE_NAME`, or
  `tools/reinstall.sh` to update an existing installation.
- Reboot
- Check the logs, `journalctl` and `systemctl status` for any failures

# Out of the Box Features
- wired networking
- wifi networking using hostapd, with radio scheduling to disable while
  you sleep and mac address randomization
- LAN switching using Linux bridge
- firewalld firewalling and port forwarding
- dnsmasq for DNS caching and DHCP
- sysctl settings to ensure the router will reboot on kernel panic or other
  kernel and hardware issues
- Automatic system updates and reboots
- systemd service management for the network configuration and all services

## Tooling
There are scripts installed to `/usr/local/bin` that perform various
adminstrative functions.  See the individual scripts, which you may also
want to customize.

