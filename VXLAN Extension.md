![[VXLAN Extension-1794026325966.webp]]


New VLAN for LabX: 100
## Our previous configuration
Here we configured A-SW1 -> C1, A-SW1 -> C2, B-SW1 -> C1, B-SW1 -> C2 as trunks for VLAN 100. 
VTEPs: C1, C2
C1 -> C2 link is layer 3.

But the problem was A-SW1 sends packet to C1. C1 already has a trunk link to B-SW1. So it can send to B-SW1. And so it can update its MAC table with