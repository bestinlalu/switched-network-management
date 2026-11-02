---
project: SNM Lab Exam
source: SNM
---
Step 1. Define the scope and constraints. Write what the project includes and excludes. Write the budget, the schedule, and the staff available. List the devices and links that already exist. 

Step 2. Write the requirements as targets. Write each business goal as a sentence. Change each goal into a target with a number or a test. Use these groups: performance, security, scalability, availability, manageability. Example: change "fast" to "4 Gbps (gigabits per second) to the Server Room". 

Step 3. Find who talks to whom. Make a table. Put the user groups in the rows. Put the destinations in the columns. Mark each cell permit, deny, or not stated. Write each not stated cell as an assumption. Calculate the traffic load of each flow. 

Step 4. Choose the structure. Divide the network into access, distribution, and core tiers. Combine tiers if the network is small. Put shared services (servers, edge routers) in their own place. 

Step 5. Do the logical design. Make one VLAN (virtual local area network) for each user group. Give each VLAN one subnet. Give each building one contiguous block of addresses. Leave unused addresses for growth. Write the routing and the security rules. 

Step 6. Ask the failure question. Select one device or link. Ask: "If it fails, who loses what?" If the answer breaks a requirement, add a second device or link. Do this for every device and link on each path. 

Step 7. Find conflicts between requirements. Compare the requirements in pairs. Ask: "Does the tool for one requirement break the other?" Example: a redundant link makes a loop, and STP (Spanning Tree Protocol) can block that link. Write the decision and the reason for each conflict. 

Step 8. Do the physical design. Place the devices. Count the ports and select the link speeds. Make sure each device has a reason from a previous step. 

Step 9. Write the tests. Write at least one test for each requirement. Example: "From a Guest host, ping a Faculty host. The ping must fail." Use the tests to prove the design in the lab. 

Step 10. Write the documents and plan for growth. Write the diagram, the address plan, and the rules. Write all assumptions. Calculate when the design needs more hardware.

