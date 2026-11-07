![[VXLAN Extension-1794026325966.webp]]


New VLAN for LabX: 100
## Our previous configuration
Here we configured A-SW1 -> C1, A-SW1 -> C2, B-SW1 -> C1, B-SW1 -> C2 as trunks for VLAN 100. 
VTEPs: C1, C2
C1 -> C2 link is layer 3.

But the problem was A-SW1 sends packet to C1. C1 already has a trunk link to B-SW1. So it can send to B-SW1. And so it can update its MAC table with that value. Also, C1 has VTEP configured for VLAN 100. So that flow also runs from C1 -> C2 -> B-SW1. This also updates the mac table. The fast change in mac addresses flag a loop and error-disables the port on C1 connected to A-SW1. 

Similar thing happens on C2 and error-disables its port connected to A-SW1. Essentially, no packet coming from A-SW1 reached C1 and C2.

## Solution

![[VXLAN Extension-1794026959629.webp]]

Since C2 is remote VTEP for A-SW1 and C1 is remote VTEP for B-SW1. So the links connecting them should not act as trunk links for VLAN 100.

Now everything works.

## Wireshark Captures

### Host to A-SW1
![[VXLAN Extension-1794027140787.webp]]

### C1 to C2
