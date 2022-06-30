# logging into condor

vpn fchampalimaud then log into htcondor
```bash
$ssh thomas.mullen@htcondor
# [Enter wifi password]
```

```bash
$pwd
>> /home/RESEARCH/thomas.mullen

```

# Access data folder

```bash
$ls /nfs/tank/

abbe                    fior                moreno         rodent-platform
admin                   fish-platform       mttp           sanchez-danes
biophotonics            flow-cytometry      oliveira       science-communication
carey                   fly-platform        oliveira-maia  scientific-hardware
ccig                    godinho-ferreira    orger          scientific-software
```

```bash
$ls /nfs/tank/orger/

\shares  users
```

# Mount synology data
```bash
$mount -t cifs //10.40.12.40/home synology-mnt

\shares  users
```

# My drive

```bash
cd /nfs/tank/orger/users/thomas.mullen/
```


