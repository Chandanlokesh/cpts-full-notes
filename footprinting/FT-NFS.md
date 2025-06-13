- network file system 
- used between linux and unix sys till nfsv3 form nfsv4 windows smb protocol was added
- this nfsv4 has support for tcp and udp at `2049`
- NFS is based on the [Open Network Computing Remote Procedure Call](https://en.wikipedia.org/wiki/Sun_RPC) (`ONC-RPC`/`SUN-RPC`) protocol exposed on `TCP` and `UDP` ports `111` and used [External Data Representation](https://en.wikipedia.org/wiki/External_Data_Representation) (`XDR`) for the system-independent exchange of data.
- no authentication or authorisation works underlying RPC protocol like UID (user id) or GID (group id)

### default configuration
- `/etc/exports` file contain a table of physical filesystem on NFS accessible by clients [NFS Exports Table](http://manpages.ubuntu.com/manpages/trusty/man5/exports.5.html)
##### ExportFS
```shell
root@nfs:~ echo '/mnt/nfs  10.129.14.0/24(sync,no_subtree_check)' >> /etc/exports


systemctl restart nfs-kernel-server

exports
```

## Footprinting the Service

```shell
sudo nmap 10.129.14.128 -p111,2049 -sV -sC --script nfs*
```

after discovered nfs share we can mount to local machine

```shell
#show all available NFS shares
showmount -e 10.129.14.128
```

```bash
#mounting NFS shares

mkdir target-NFS

sudo mount -t nfs 10.129.14.128:/ ./targets-NFS/ -o nolock

cd target-NFS

tree .
```

```bash
#list contents with uname and group names
ls -l mnt/nfs

#list contents with uid and guid
ls -n mnt/nfs
```

if we have root also we cant edit files . so we can upload shell to NFS share that has the SUID of the user and then run the shell via the ssh user

##### unmounting
```bash
cd ..
sudo unmount ./target-NFS
```