There are many types of archive files. Some of the more commonly encountered file extensions include `tar`, `gz`, `rar`, `zip`, `vmdb/vmx`, `cpt`, `truecrypt`, `bitlocker`, `kdbx`, `deb`, `7z`, and `gzip`.

## identify archive files online
```shell-session
curl -s https://fileinfo.com/filetypes/compressed | html2text | awk '{print tolower($1)}' | grep "\." | tee -a compressed_ext.txt
```

## Cracking ZIP files
```bash
zip2john ZIP.zip >zip.hash
cat zip.hash

john --wordlist=rockyou.txt zip.hash
john zip.hash --show
```

## Cracking OpenSSL encrypted GZIP files

we can find the file is password protected or not 
```bash
file GZIP.gzip

```

some time we will get false positives or complete failure to identify the correct password 
we can use for loop to extract correct password then only give the successfull message 

```shell
for i in $(cat rockyou.txt);do openssl enc -aes-256-cbc -d -in GZIP.gzip -k $i 2>/dev/null| tar xz;done
```
we can check the current dir new file

## Cracking BitLocker-encrypted drives
[BitLocker](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-device-encryption-overview-windows-10) is a full-disk encryption feature developed by Microsoft for the Windows operating system
- uses ASE encryption algo either 128-bit or 256-bit key length and if we forgot the pass we can use recovery key of 48-digit string  

To crack a BitLocker encrypted drive, we can use a script called `bitlocker2john` to [four different hashes](https://openwall.info/wiki/john/OpenCL-BitLocker): the first two correspond to the BitLocker password, while the latter two represent the recovery key. Because the recovery key is very long and randomly generated, it is generally not practical to guess—unless partial knowledge is available

so we will focus on cracking the password using first hash (`$bitlocker$0$...`).

```bash
bitlocker2john -i Backup.vhd > backup.hashes
gerp "bitlocker\$0" backup.hashes > backup.hash
cat backup.hash
```

```shell
hashcat -a 0 -m 22100 '$bitlocker$0$16$02b329c0453b9273f2fc1b927443b5fe$1048576$12$00b0a67f961dd80103000000$60$d59f37e70696f7eab6b8f95ae93bd53f3f7067d5e33c0394b3d8e2d1fdb885cb86c1b978f6cc12ed26de0889cd2196b0510bbcd2a8c89187ba8ec54f' /usr/share/wordlists/rockyou.txt
```

#### Mounting BitLocker-encrypted drives in Windows
- double click on `.vhd` file then showing an error initially then after mounting double click on it and enter the password 

#### Mounting BitLocker-encrypted drives in Linux (or macOS)

```shell-session
sudo apt-get install dislocker
```
```bash
sudo mkdir -p /media/bitlocker
sudo mkdir -p /media/bitlockermount
```
We then use `losetup` to configure the VHD as [loop device](https://en.wikipedia.org/wiki/Loop_device), decrypt the drive using `dislocker`, and finally mount the decrypted volume:
```bash
sudo lostup -f -P Backup.vhd
sudo dislocker /dev/loop0p2 -u1234qwer --/media/bitlocker
sudo mount -o loop /media/bitlocker/dislocker-file /media/bitlockermount

cd /media/bitlockermount/
ls -la
```