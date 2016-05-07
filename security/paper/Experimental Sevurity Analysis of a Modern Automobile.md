###<center>Experimental Security Analysis of a Modern Automobile###
####OverView####
Modern automotives are no longer mere mechanical devices, they are controled by dozens of digital computers via internal networks. While this transformation has driven major advancements in efficiency and safety, it has also introduced a range of new potential risks. This paper evaluated these issue on a modern automobile and experimentally demonstrated the fragility of the components.
####Background####
Digital control, in the form of self-contained embedded systems called Engine Control Unit(ECU), has entered into automobile in 1970s. By dynamically measuring the oxygen present in exhaust fumes, the ECU could then adjust the fuel/oxygen mixture before combustion, thereby improving efficientcy and reducing pollutants. Since then, such system has been integrated into virtually every aspect of a car's fuctioning and diagnostics, causing the term ECU to be generalized to *Electronic Control Units*.
Many features require complex interactions across ECUS. Some early systems used one-off designs and bilateral physical wire connections for such interactions, but this approch does not scale. A combination of time-to-market pressures, wiring overhead, interaction complexity and economy of pressures has driven manufactures and suppliers to standardize on a few key digital buses, such as CAN. CAN is a typical multiple bus covering different component groups where a high-speed bus interconnects components that generate real-time telemetry while low-speed bus controls trivial data. While it seems that such buses could be physically isolated, in practice they are "bridged" to support subtle interation requirements. Starting from mid-1990s, automotives manufactures married more powerful ECUs,providing Unix-like environment, with peripherals. There are many reasons to reconsider the state of vehicular computer security.
There are several methods for an attacker to get the access to automobiles' internal networks. The first is physical access. Someone can insert a malicious component into a car's internal network via OBD-II port. A similar entry point is presented by counterfeit or malicious components entering the vehicle parts supply chain, such as third-party components. The second vector is via numerous wireless interfaces implemented in the modern automobiles. This paper is based on the assumption that an attacker has got the access to automobiles.
####Security of CAN bus####
Since 2008, CAN as the communication network has become a standard for in-car network. However, there are some inherent weaklesses. 
 1. A CAN packet does not include addresses and instead supports a publish-and-subscribe communication model. This feature makes it exposed to the risk of DOS attack and gives attacker some potential opportunities to communicate with ECMs.l
 2. CAN contains two types of networks, high-speed network for secure and real-time data and low-speed network for normal data. When necessary, a gateway bridge can route delected data between the two buses, which provides a path for attacker to access secure data from normal space.
 3. CAN packets contain no authenticator field--or even any source identifier field--meaning that any component can indistinguishably send a packet to any other component.
 4. CAN specifies a challenge-response sequence to protect ECUs against certain actions without authorization. But this access control method is so weak that can be easily cracked.
 5. In order to upgrade the fireware of ECMs, manufactures prefer exposing ECUs to reflashing and diagnostic testing. However, it is also well know that attacker can use software updates to inject malicious code into system.

####Attack Methodology####
Packet Sniffing and Targeted Probing. This paper used a tool called CARSHARK to observe traffic on the CAN buses in order to determine how ECUs communicate with each other. And then, they can find the how to control some component in normal world.
Fuzzing. Due to the range of calid CAN packets is rather small, significant damage can be done by simple fuzzing of packets.
Reverse-Engineering. For a small of ECUs they dumped their code via CAN ReadMemory service and used a third-party debugger to explicitly understand how certain hardware features were controlled.







<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />