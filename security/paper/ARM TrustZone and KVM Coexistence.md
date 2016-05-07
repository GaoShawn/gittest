##<center>ARM TrustZone and KVM Coexistence with RTOS For Automotive###
###1. TrustZone###
####Overview####
ARM TrustZone is a hardware based technology to enhance the security that is used on billions of chips. This new architecture allows CPU to run in two different world currently, the normal world and security world, which are hardware isolated from each other. As a example within a CPU, software either resides in the secure world or the normal world with a switch between these two worlds accomplished by a secure monitor. But, how does it work. [more detail](http://blog.csdn.net/hanmengaidudu/article/details/46660257)
####Hardware Support####
TrustZone extends the AXI bus by adding a control line, which is used to send the world type signal. Besides, TrustZone adds two controlers TZASC and TZMA, managing the access to memory and devices, respectively.
####Swtich between two World####
TrustZone introduce a monitor mode, which is responsible for the switch bwtween two world. When Apps in normal world try to enter security world, it will execute a dedicated secure monitor call instruction(SMC) and trigger the secure monitor. The monitor code typically save the state of the current world and restore the state of the world being switched to. On the other hand, some exception mechanisms also can trigger the switch.

###2. KVM Virtural Machine###
####Overview on Virtural Machine####
Virtualization refers to the act of creating several virtual version of computer resources, such as CPUs, memories and peripheral devices, in a singal set of actual hardwares. Each set of vertual resources acting like a real computer that run the operating system independently, which is very helpful to make full use of the limited resources. Different from QEMU using emulation, KVM uses processor extensions (HVM) for virtualization where the codes are excuted in real processor. Virtualization can be devided into two categories, full virtualization and half virtualization.<br />
Full virtualization, such as VMware, provides platforms for operating system to run without any change, while half virtualization need some change in the kernel of guest OS. However, performance is an important problem for full virtualization. As we all know, there are four priorities in windows, ring 0 to ring 3, and userspace programs run in ring 3 while kernel space programs in ring 0. As for the guest OS which are regarded as a ordinary process by the host OS, kernel space can't occupy ring 0 as usual. An exceptiones will be triggered when the guest OS tries to visit system resource(due to the limits of authority), and the host OS will cache those exceptions and return a successive signal to guest, which is very costly.
####Hardware Support####
To deal with those problems, some CPUs(intel VT) provide special support by add VMX root and VMX noroot model.
####KVM####
KVM is a full virtualization solution for linux containing virtualization extentsions(intel VT ot AMD-V). It consists of a loadable kernel module, that provides the core virtualization infrastucture and a processor specific module. Using KVM, you can run multiple virtual machines with unmodified Linux or Windows images. Each virtual machine has private virtualized hardware: a network card, disk, graphics adapter, etc.
###3. Different between TrustZone and KVM###
Although TrustZone and KVM make it possible to run several OS in a single set of hardware, these are designed for different purpose. Virtualization is used to reuse the computer resources and build a isolated envionment, where TrustZone is design for security goals by limiting the access to computer resources of different OS.[more detail](http://www.qinyu.org/2015/09/03/isolation-consideration-20150903/)
