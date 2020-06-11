# vim 命令

1. 全局替换

   全局替换命令为：:%s/源字符串/目的字符串/g

   ```
   %s/source/target/g
   ```

2. grep 命令

   ```
   grep -rn coupon ./^
   ```

3. cat

   cat | cut -c 1-4 idfa_2020-03-01.txt |grep  -c '1234'

   cat | cut -c 1-4 idfa_2020-03-01.txt |grep  -c '1234'



# top参数

Fields Management for window 1:Def, whose current sort field is %MEM
   Navigate with Up/Dn, Right selects for move then <Enter> or Left commits,
   'd' or <Space> toggles display, 's' sets sort.  Use 'q' or <Esc> to end!

* PID     = Process Id         
* USER    = Effective User Name 
* PR      = Priority            ：优先级 
* NI      = Nice Value          
* VIRT    = Virtual Image (KiB) 
* RES     = Resident Size (KiB) 
* SHR     = Shared Memory (KiB) 
* S       = Process Status      
* %CPU    = CPU Usage           
* %MEM    = Memory Usage (RES)  
* TIME+   = CPU Time, hundredths
* COMMAND = Command Name/Line   
  PPID    = Parent Process pid  
  UID     = Effective User Id   
  RUID    = Real User Id        
  RUSER   = Real User Name      
  SUID    = Saved User Id       
  SUSER   = Saved User Name     
  GID     = Group Id            
  GROUP   = Group Name          
  PGRP    = Process Group Id    
  TTY     = Controlling Tty     
  TPGID   = Tty Process Grp Id  
  SID     = Session Id          
  nTH     = Number of Threads   
  P       = Last Used Cpu (SMP) 
  TIME    = CPU Time            
  SWAP    = Swapped Size (KiB)  
  CODE    = Code Size (KiB)     
  DATA    = Data+Stack (KiB)    
  nMaj    = Major Page Faults   
  nMin    = Minor Page Faults   
  nDRT    = Dirty Pages Count   
  WCHAN   = Sleeping in Function
  Flags   = Task Flags <sched.h>
  CGROUPS = Control Groups      
  SUPGIDS = Supp Groups IDs     
  SUPGRPS = Supp Groups Names   
  TGID    = Thread Group Id     
  ENVIRON = Environment vars    
  vMj     = Major Faults delta  
  vMn     = Minor Faults delta  
  USED    = Res+Swap Size (KiB) 
  nsIPC   = IPC namespace Inode 
  nsMNT   = MNT namespace Inode 
  nsNET   = NET namespace Inode 
  nsPID   = PID namespace Inode 
  nsUSER  = USER namespace Inode
  nsUTS   = UTS namespace Inode 

# ps

https://www.cnblogs.com/huchong/p/10065246.html#_label0