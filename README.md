<h1 align="center">SSH Ramdisk Script</h1>
<p align="center">
  <a href="https://github.com/verygenericname/SSHRD_Script/graphs/contributors" target="_blank">
    <img src="https://img.shields.io/github/contributors/verygenericname/SSHRD_Script.svg" alt="Contributors">
  </a>
  <a href="https://github.com/verygenericname/SSHRD_Script/commits/main" target="_blank">
    <img src="https://img.shields.io/github/commit-activity/w/verygenericname/SSHRD_Script.svg" alt="Commits">
  </a>
</p>

<p align="center">
Create and boot a SSH ramdisk on checkm8 devices
</p>

---

# Prerequsites

1. A computer running macOS/linux
2. A checkm8 device (A7-A11)

# Usage

1. Clone and cd into this repository: `git clone https://github.com/Lawin0129/SSHRD_Script --recursive && cd SSHRD_Script`
    - If you have cloned this before, run `cd SSHRD_Script && git pull` to pull new changes
2. Run `sudo ./sshrd.sh <iOS version for ramdisk>`, **without** the `<>`.
    - If your device is on iOS 11 or under, pick 12.0 for the ramdisk version. Otherwise, use the current iOS version installed on your device.
    - If you're on Linux, you will not be able to make a ramdisk for 16.1+, please use something lower instead, like 16.0
        - This is due to ramdisks switching to APFS over HFS+, and another dmg library would have to be used
3. Place your device into DFU mode
    - A11 users, go to recovery first, then DFU.
4. Run `sudo ./sshrd.sh boot` to boot the ramdisk
5. Run `sudo ./sshrd.sh ssh` to connect to SSH on your device
6. Finally, to mount the filesystems,
    - For iOS 7.0 - 10.2.1:
        - Run `mount_hfs /dev/disk0s1s1 /mnt1` to mount the RootFS (iOS Firmware) partition on /mnt1
        - Run `mount_hfs /dev/disk0s1s2 /mnt2` to mount the UserData partition on /mnt2
    - For iOS 10.3 and above:
        - Run `mount_apfs /dev/disk0s1s1 /mnt1` to mount the RootFS (iOS Firmware) partition on /mnt1
        - Run `mount_apfs /dev/disk0s1s2 /mnt2` to mount the UserData partition on /mnt2
    - For iOS 11 and above, you can also try running `mount_filesystems` which mounts every partition. Do not run if below iOS 11.
7. Have fun!

# Dumping Nand
1. Follow [Usage](https://github.com/Lawin0129/SSHRD_Script?tab=readme-ov-file#usage) up to step 4.
2. Once you've booted the ramdisk, run `./sshrd.sh dump-nand`
    - You can also dump specific partitions,
        - Run `sudo ./sshrd.sh dump-mnt1` to dump the whole RootFS (disk0s1s1)
        - Run `sudo ./sshrd.sh dump-mnt2` to dump the whole user data partition (disk0s1s2)
3. It should now start dumping. After disk0 is dumped, it will ask if you want to dump the specific partitions for any reason you might want them (disk0 should contain everything though).
4. The dumps will be saved in the current directory with the file names `disk0.gz`, `disk0s1s1.gz`, `disk0s1s2.gz`
5. Once everything is done, your iDevice will reboot into Recovery Mode. Run `sudo ./sshrd.sh fix-auto-boot` to kick it out of Recovery Mode.

# Linux notes

On Linux, usbmuxd will have to be restarted. On most distros, it's as simple as these 2 commands in another terminal:
```
sudo systemctl stop usbmuxd
sudo usbmuxd -p -f
```

# Other commands

- Reboot your device: `sudo ./sshrd.sh reboot`
- Erase all data from your device: `sudo ./sshrd.sh reset`
- Fixes auto-boot on your device: `sudo ./sshrd.sh fix-auto-boot`
- Dump onboard SHSH blobs: `sudo ./sshrd.sh dump-blobs`
- Dump ENTIRE contents of your device: `sudo ./sshrd.sh dump-nand`
- Dump mnt1 of your device: `sudo ./sshrd.sh dump-mnt1`
- Dump mnt2 of your device: `sudo ./sshrd.sh dump-mnt2`
- Restores nand dump to your device: `sudo ./sshrd.sh restore-nand`
- Restores mnt1 dump to your device: `sudo ./sshrd.sh restore-mnt1`
- Restores mnt2 dump to your device: `sudo ./sshrd.sh restore-mnt2`
- Delete old SSH ramdisk: `sudo ./sshrd.sh clean`

# Other Stuff

- [Reddit Post](https://www.reddit.com/r/jailbreak/comments/wgiye1/free_release_ssh_ramdisk_creator_for_iphones_ipad/)

# Credits

- [tihmstar](https://github.com/tihmstar) for pzb/original iBoot64Patcher/img4tool
- [xerub](https://github.com/xerub) for img4lib and restored_external in the ramdisk
- [Cryptic](https://github.com/Cryptiiiic) for iBoot64Patcher fork
- [opa334](https://github.com/opa334) for TrollStore
- [Nebula](https://github.com/itsnebulalol) for a bunch of QOL fixes to this script
- [OpenAI](https://chat.openai.com/chat) for converting [kerneldiff](https://github.com/mcg29/kerneldiff) into [C](https://github.com/verygenericname/kerneldiff_C)
- [Ploosh](https://github.com/plooshi) for KPlooshFinder
