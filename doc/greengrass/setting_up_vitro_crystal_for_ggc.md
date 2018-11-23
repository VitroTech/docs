# Setting up a Vitro Crystal for AWS Greengrass

This guide presents how to setup Vitro Crystal device with Vitrobian 0.1.0
for AWS Greengrass.

1. Download Vitrobian operating system.

    [Download link](https://s3-eu-west-1.amazonaws.com/prod-vitrobian-releases/vitrobian_0.1.0.img.gz)

2. Verify downloaded image.

    Use either `md5sum` or `sha256sum` commands to check the image integrity.
    Outputs of both of them are presented below:

    * `md5sum`:

    ```
    $ md5sum vitrobian_0.1.0.img.gz
    aa05de9a10d22a0995a1dd8adfdaf8a1  vitrobian_0.1.0.img.gz
    ```

    * `sha256sum`:

    ```
    $ sha256sum vitrobian_0.1.0.img.gz
    da286e074b5a4e151780dce5ddfce2b452d82be37d632db5e86828941aca1d30  vitrobian_0.1.0.img.gz
    ```

3. Flash the system into SD card.

    Insert the SD card to the SD card reader of your host PC. Then pick one of the
    methods below to flash the SD card. The first method (GUI) is preferred.

* using simple GUI

    Download [etcher](https://etcher.io/). It is multi-platform application that
    is available for Linux, Windows or macOS.

    Click on `Select Image` and select `vitrobian_0.1.0.img.gz`.
    There is no need to unpack the image first.

    The SD card reader should be picked automatically. If more than one readers
    are present, click on `Change` and select the one you want to use.

    Configured application window should look similar to this one (SD card
    reader name may vary):

    ![etcher](etcher.png)

    When confirmed, click on the `Flash` button to start flashing procedure.
    Depending on your setup, you will be prompted for `sudo user` or
    `administrator` password.

* using command line

    Determine the path of your SD card executing the command:
    ```
    $ sudo fdisk -l
    ```

    Size of the SD card can help you to be sure that it is the correct device.
    Exemplary output of above command:

    ```
    Disk /dev/sdb: 465,8 GiB, 500107862016 bytes, 976773168 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disklabel type: dos
    Disk identifier: 0x0351db27

    Device     Boot     Start       End   Sectors   Size Id Type
    /dev/sdb1  *         2048 164100095 164098048  78,3G  7 HPFS/NTFS/exFAT
    /dev/sdb2       164102142 976771071 812668930 387,5G  5 Extended
    /dev/sdb5       164102144 943476735 779374592 371,7G 83 Linux
    /dev/sdb6       943478784 976771071  33292288  15,9G 82 Linux swap / Solaris


    Disk /dev/sdc: 14,9 GiB, 16021192704 bytes, 31291392 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disklabel type: dos
    Disk identifier: 0x0590b0fb

    Device     Boot Start     End Sectors Size Id Type
    /dev/sdc1  *     2048   34815   32768  16M  c W95 FAT32 (LBA)
    /dev/sdc2       34816 2131967 2097152   1G 83 Linux
    ```

    In this case the SD card is `/dev/sdc`. Unmount the card using command:

    ```
    $ sudo umount /dev/<your-sd-card>*
    ```

    Replace `<your-sd-card>` with name of your SD card determined in the
    previous step, e.g:

    ```
    $ sudo umount /dev/sdc*
    ```

    Flash the image into SD card by executing:

    ```
    $ gzip -cdk <image-file-name> | sudo dd of=/dev/<your-sd-card> bs=16M
    ```

    Set to correct `<your-sd-card>` and `<image-file-name>` to the path of the
    system image downloaded in the first step, e.g:

    ```
    $ gzip -cdk vitrobian_0.1.0.img.gz | sudo dd of=/dev/sdc bs=16M
    ```

4. Prepare Vitro Crystal platform

    * Attach jumper on the `J12` header as shown on the photo:

    ![boot_select_jumper](boot_select_jumper.png)

    * Setup the boot configuration switches (SW2) as shown on the photo:

    ![boot_select_switch](boot_select_switch.png)

    * Insert the SD card into the Vitro Crystal.

    * Connect Ethernet cable and power supply for the board

    ![connected_board](connected_board.png)

5. Connect to Vitro Crystal using serial port.

    Connect Vitro Crystal to your computer via micro USB cable using J5
    connector near micro SD card slot. Establish serial connection using e.g.
    `minicom`. The communication parameters are: `115200 8N1`.

    An example command to establish connection using `minicom`:

    ```
    minicom -b 115200 -o -D /dev/ttyUSB0
    ```

    or `screen`


    ```
    screen /dev/ttyUSB0 115200
    ```

    > You may need to extend your privileges to use serial port, i.e. use
    > `sudo` to run those commands.

6. Login to Vitrobian with credentials:

    ```
    login: user
    pass: user
    ```

7. Determine the IP address of your Vitro Crystal.

    IP address can be determined by simply ping command from your host computer:

    ```
    $ ping crystal.local
    ```

    Output should be similar to below:

    ```
    PING crystal.local (192.168.4.184) 56(84) bytes of data.
    64 bytes from 192.168.4.184 (192.168.4.184): icmp_seq=1 ttl=64 time=0.205 ms
    ```

    Alternatively you can execute the following command on Vitro Crystal
    when you are connected via serial port:

    ```
    $ /sbin/ifconfig
    ```

    Exemplary output of above command:

    ```
    eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
    inet 192.168.3.124  netmask 255.255.255.0  broadcast 192.168.3.255
    inet6 fe80::385e:94ff:fe71:2ba3  prefixlen 64  scopeid 0x20<link>
    ether 3a:5e:94:71:2b:a3  txqueuelen 1000  (Ethernet)
    RX packets 4556  bytes 675957 (660.1 KiB)
    RX errors 0  dropped 0  overruns 0  frame 0
    TX packets 414  bytes 54149 (52.8 KiB)
    TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

    lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
    inet 127.0.0.1  netmask 255.0.0.0
    inet6 ::1  prefixlen 128  scopeid 0x10<host>
    loop  txqueuelen 1  (Local Loopback)
    RX packets 2  bytes 78 (78.0 B)
    RX errors 0  dropped 0  overruns 0  frame 0
    TX packets 2  bytes 78 (78.0 B)
    TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    ```

8. Connect to your Vitro Crystal using SSH.

    Execute following command on your computer:

    ```
    $ ssh user@<vitro-crystal-ip-address>
    ```

    Please, change the `vitro-crystal-ip-address` to address obtained in
    previous step.

    Login to Vitro Crystal using credentials from point: `7. Login to Vitrobian
    with credentials`.

9. Create user and group for GGC.

    Execute following commands on Vitro Crystal:

    ```
    $ sudo adduser --system ggc_user
    $ sudo addgroup --system ggc_group
    ```

12. Enable hardlink and softlink protection.

    It is recommended to activate this protection to improve security on the
    device. In order to do that please uncomment and set to `1` following lines 
    in file `/etc/sysctl.d/99-sysctl.conf`:

    ```
    #fs.protected_hardlinks=0
    #fs.protected_symlinks=0
    ```

    These are the last two lines of the file.
    After changed reboot the platform by typing:

    ```
    $ sudo /sbin/reboot
    ```

    Then confirm changes using command:

    ```
    $ sudo sysctl -a | grep fs.protected
    ```

    The output should contain:

    ```
    fs.protected_hardlinks = 1
    fs.protected_symlinks = 1
    ```

10. Install required packages

    ```
    $ sudo apt install git wget
    ```

11. Verify your Vitro Crystal system.

    Now the Vitro Crystal platform should be ready for AWS Greengrass. You can
    additionally verify your system using AWS Greengrass dependency checker,
    which can be downloaded from [AWS GitHub repository](https://github.com/aws-samples/aws-greengrass-samples).

    ```
    $ git clone https://github.com/aws-samples/aws-greengrass-samples.git
    ```

    And run dependency checker:

    ```
    $ cd aws-greengrass-samples/greengrass-dependency-checker-GGCv1.6.0
    $ sudo ./check_ggc_dependencies
    ```

    Output should look like below:
    ```
    ==========================Checking script dependencies==============================
    The device has all commands required for the script to run.

    ========================Dependency check report for GGC v1.6=========================
    System configuration:
    Kernel architecture: armv7l
    Init process: /usr/lib/systemd/systemd
    Kernel version: 4.9
    C library: Debian GLIBC 2.24-11+deb9u3
    C library version: 2.24
    Directory /var/run: Present
    /dev/stdin: Found
    /dev/stdout: Found
    /dev/stderr: Found

    --------------------------------Kernel configuration--------------------------------
    Kernel config file: /boot/config-4.9.0-7-armmp

    Namespace configs:
    CONFIG_IPC_NS: Enabled
    CONFIG_UTS_NS: Enabled
    CONFIG_USER_NS: Enabled
    CONFIG_PID_NS: Enabled

    Cgroup configs:
    CONFIG_CGROUP_DEVICE: Enabled
    CONFIG_CGROUPS: Enabled
    CONFIG_MEMCG: Enabled

    Other required configs:
    CONFIG_POSIX_MQUEUE: Enabled
    CONFIG_OVERLAY_FS: Enabled
    CONFIG_HAVE_ARCH_SECCOMP_FILTER: Enabled
    CONFIG_SECCOMP_FILTER: Enabled
    CONFIG_KEYS: Enabled
    CONFIG_SECCOMP: Enabled

    ------------------------------------Cgroups check-----------------------------------
    Cgroups mount directory: /sys/fs/cgroup

    Devices cgroup: Enabled and Mounted
    Memory cgroup: Enabled and Mounted

    ----------------------------Commands and software packages--------------------------
    Python version: 2.7.13
    NodeJS 6.10: Not found
    Java 8: Not found
    OpenSSL version: 1.0.2
    wget: Present
    realpath: Present
    tar: Present
    readlink: Present
    basename: Present
    dirname: Present
    pidof: Present
    df: Present
    grep: Present
    umount: Present

    ---------------------------------Platform security----------------------------------
    Hardlinks_protection: Enabled
    Symlinks protection: Enabled

    -----------------------------------User and group-----------------------------------
    ggc_user: Present
    ggc_group: Present

    ------------------------------------Results-----------------------------------------
    Note:
    1. It looks like the kernel uses 'systemd' as the init process. Be sure to set the
    'useSystemd' field in the file 'config.json' to 'yes' when configuring Greengrass core.

    Missing optional dependencies:
    1. Could not find the binary 'nodejs6.10'.

    If NodeJS 6.10 or later is installed on the device, name the binary 'nodejs6.10' and
    add its parent directory to the PATH environment variable. NodeJS 6.10 or later is
    required to execute NodeJS lambdas on Greengrass core.

    2. Could not find the binary 'java8'.

    If Java 8 or later is installed on the device name the binary 'java8' and add its
    parent directory to the PATH environment variable. Java 8 or later is required to
    execute Java lambdas on Greengrass core.


    ----------------------------------Exit status---------------------------------------
    You can now proceed to installing the Greengrass core 1.6 software on the device.
    Please reach out to the AWS Greengrass support if issues arise.
    ```

    If above output match, you can continue with installing Greengrass Core on
    your Vitrobian using official [AWS guide](http://docs.aws.amazon.com/greengrass/latest/developerguide/module2.html).
