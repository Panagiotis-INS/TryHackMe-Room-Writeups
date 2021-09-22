# Author:Panagiotis Fiskilis/Neuro

## Room name:Try Hack Me:Memory Forensics ##

### Flags: ###

**Task1:Introduction**

- Q:I have understood the task and can continue to the questions!

A:```No answer needed```

**Task2:Login**

- Q:What is John's password?

A:```charmander999```

```bash
volatility -f Snapshot6.vmem imageinfo
volatility -f Snapshot6.vmem --profile=Win7SP1x64 hashdump |tee hashdump.log
john --format=NT --wordlist=/opt/1337/rockyou.txt hashdump.log
```

***NOTE:*** ```--profile=Win7SP1x64```

**Task3:Analysis**

- Q:When was the machine last shutdown?

A:```2020-12-27 22:50:12```

```bash
volatility -f Snapshot19.vmem imageinfo
#hardcore/kinky
volatility -f Snapshot19.vmem --profile=Win7SP1x64 timeliner |tee timeliner.log #hardcore/kinky
#Kinda normal
volatility -f Snapshot19.vmem --profile=Win7SP1x64 hivelist
volatility -f Snapshot19.vmem --profile=Win7SP1x64 printkey -o 0xfffff8a000024010 -K "ControlSet001\Control\Windows"
#normal
volatility -f Snapshot19.vmem --profile=Win7SP1x64 shutdowntime
```

- Q:What did John write?

A:```You_found_me```

```bash
volatility -f Snapshot19.vmem --profile=Win7SP1x64 cmdline |tee cmdline.log
volatility -f Snapshot19.vmem --profile=Win7SP1x64 cmdscan |tee cmdscan.log
volatility -f Snapshot19.vmem --profile=Win7SP1x64 consoles |tee consoles.log
```

**Task4:TrueCrypt**

- Q:What is the TrueCrypt passphrase?

A:```forgetmenot```

```bash
volatility -f Snapshot14.vmem imageinfo
volatility -f Snapshot14.vmem --profile=Win7SP1x64 truecryptpassphrase
```
