# CentOS

## 新磁碟掛載
>> ### 尋找磁碟
>>     df -h
>> 顯示目前硬碟掛載狀態
>> ```
>> Filesystem            Size  Used Avail Use% Mounted on
>> /dev/sda1             440G   46G  372G  12% /
>> none                  5.9G  260K  5.9G   1% /dev
>> none                  5.9G     0  5.9G   0% /dev/shm
>> none                  5.9G   64K  5.9G   1% /var/run
>> none                  5.9G     0  5.9G   0% /var/lock
>> none                  5.9G     0  5.9G   0% /lib/init/rw
>> /dev/sdb1             459G  198M  435G   1% /data1
>> ```
>> 
>>     ls /dev/[sh]d*
>> 顯示所有硬碟狀況
>> ```
>> /dev/sda  /dev/sda1  /dev/sda2  /dev/sda5  /dev/sdb  /dev/sdb1  /dev/sdc
>> ```
>
>> ### 分割磁區
>>     fdisk -l /dev/sdc
>> 查看硬碟資訊，`sdc` 剛剛查的新磁區名稱
>> ```
>> Disk /dev/sdc: 2000.4 GB, 2000398934016 bytes
>> 255 heads, 63 sectors/track, 243201 cylinders
>> Units = cylinders of 16065 * 512 = 8225280 bytes
>> Sector size (logical/physical): 512 bytes / 512 bytes
>> I/O size (minimum/optimal): 512 bytes / 512 bytes
>> Disk identifier: 0x40bab849
>>    Device Boot      Start         End      Blocks   Id  System
>> (這裡沒資料代表這顆磁碟沒配置)
>> ```
>> 
>>     fdisk /dev/sdc
>> 分割磁碟，`sdc` 剛剛查的新磁區名稱
>> 
>> 輸入 `m` 在按 `Enter` 可以顯示各種指令的說明：
>> 
>> ```
>> WARNING: DOS-compatible mode is deprecated. It’s strongly recommended to
>>          switch off the mode (command ‘c’) and change display units to
>>          sectors (command ‘u’).
>> Command (m for help): m
>> Command action
>>    a   toggle a bootable flag
>>    b   edit bsd disklabel
>>    c   toggle the dos compatibility flag
>>    d   delete a partition
>>    l   list known partition types
>>    m   print this menu
>>    n   add a new partition
>>    o   create a new empty DOS partition table
>>    p   print the partition table
>>    q   quit without saving changes
>>    s   create a new empty Sun disklabel
>>    t   change a partition’s system id
>>    u   change display/entry units
>>    v   verify the partition table
>>    w   write table to disk and exit
>>    x   extra functionality (experts only)
>> Command (m for help): 
>> ```
>> 新增分割區，輸入 `n` 按 `Enter`。
>> 
>> 選擇要建立 extended 還是 primary partition，因為我的硬碟全部只要一個分割區，所以我選 primary，輸入 `p` 按 `Enter`。
>> 
>> 選擇 Partition number，primary 分割區最多可以有四個，隨便選都可以，不過建議選 1，免得以後看起來很奇怪，輸入 `1` 按 `Enter`。
>> 
>> 輸入開始的 cylinder，用預設值就可以了，直接按 `Enter`。
>> 
>> 輸入結束的 cylinder，若是要用最大的容量，就直接按 `Enter`，若是要指定分割區的大小，就用 +size{K,M,G} 的形式指定，例如指定為 100G 的大小就輸入 +100G `(指定結束磁區，+100G不一定能使用)`再按 `Enter`。
>> 
>> 最後將分割表寫入硬碟，輸入 `w` 再按 `Enter`。
>> 
>> ```
>> Command (m for help): n
>> Command action
>>    e   extended
>>    p   primary partition (1-4)
>> p
>> Partition number (1-4): 1
>> First cylinder (1-243201, default 1):
>> Using default value 1
>> Last cylinder, +cylinders or +size{K,M,G} (1-243201, default 243201):
>> Using default value 243201
>> Command (m for help): w
>> The partition table has been altered!
>> Calling ioctl() to re-read partition table.
>> Syncing disks.
>> ```
>> 若是要離開 fdisk 就輸入 `q` 按 `Enter` 就可以了。
>> 
>>     fdisk -l /dev/sdc
>> 接著再用fdisk確認分割區
>> ```
>> Disk /dev/sdc: 2000.4 GB, 2000398934016 bytes
>> 255 heads, 63 sectors/track, 243201 cylinders
>> Units = cylinders of 16065 * 512 = 8225280 bytes
>> Sector size (logical/physical): 512 bytes / 512 bytes
>> I/O size (minimum/optimal): 512 bytes / 512 bytes
>> Disk identifier: 0x40bab849
>>    Device Boot      Start         End      Blocks   Id  System
>> /dev/sdc1               1      243201  1953512001   83  Linux
>> ```
> 
>> ### 格式化
>>     mkfs.xfs /dev/sdc1
> 
>> ### 建立目錄
>>     mkdir /new-sd-10G
> 
>> ### 掛載磁碟
>>     mount /dev/sdc1 /new-sd-10G
>> 掛載磁區 `/dev/sdc1` 到指定目錄 `/new-sd-10G` 下
>> 

### 參考
[CentOS 7 下 fdisk 對掛載新硬碟的操作 - IT閱讀](https://www.itread01.com/content/1541147583.html)