<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, I conduct experiments with various network traffic to and from Azure Virtual Machines. I also carry out trials and tests with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Command-Line Tools
- Various Network Protocols


<h2>Operating Systems Used </h2>

- Windows 10 (22H2)
- Windows Server 2022
<h2>High-Level Steps</h2>

- Create a DNS A-record
- Observe DNS Cache
- Flush DNS Cashe and observe changes
- Create an array of file shares with varying permissions
- Attempt to access file shares and validate restrictions
- Create a Security Group, assign permissions, test access
  
<h2>Actions and Observations</h2>


<p>
<img width="1660" alt="Screenshot 2023-08-22 at 9 04 27 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/f57ccc9b-b78b-47fa-8088-6edbec7a80e2">
</p>
<p>
I am logged into the domain controller and in the DNS Manager.
</p>
<br />

<p>
<img width="1616" alt="Screenshot 2023-08-22 at 9 05 08 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/a9675f38-59aa-42d4-979a-5f94c3b5eeba">
</p>
<p>
I create a DNS A-record on the domain controller as “mainframe” and have it point to the domain controller's private IP address.
</p>
<br />

<p>
<img width="1512" alt="Screenshot 2023-08-22 at 9 07 45 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/5ba8abe2-e572-4d2c-a4b6-2aba0e00acd2">
</p>
<p>
In this screenshot, I am logged into the client and ping "mainframe" and see that it works.
</p>
<br />

<p>
<img width="1512" alt="Screenshot 2023-08-22 at 9 08 56 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/1aa25a1f-ed83-41a3-a36d-532cd02d7590">
</p>
<p>
ping mainframe
</p>
<br />

<p>
<img width="1512" alt="Screenshot 2023-08-22 at 9 09 11 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/3ff1934c-8f6f-46da-842a-eea88495618f">
</p>
<p>
nslookup mainframe
</p>
<br />

<p>
<img width="1670" alt="Screenshot 2023-08-22 at 9 10 48 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/ed21788e-fedb-4e81-b5a0-f01564168ab2">
</p>
<p>
ipconfig /displaydns you can see the corresponding details of "mainframe"
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 9 12 09 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/db24085d-32fb-49db-a236-e697ad3440e9">
</p>
<p>
I am logged back into the DNS manager.
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 9 12 52 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/bfff14c0-8171-4f5b-805c-e9d9389cd42a">
</p>
<p>
I change mainframe’s record address to 8.8.8.8
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 9 13 01 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/dc0f8d78-1bd1-4ce4-9dba-2b108d7b9c7d">
</p>
<p>
You can see the confirmed change of "mainframe" IP address to 8.8.8.8
</p>
<br />

<p>
<img width="1670" alt="Screenshot 2023-08-22 at 9 14 10 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/493169f0-0c64-48de-8cc0-240631c748de">
</p>
<p>
As part of the DNS cache exercise, I ping "mainframe" from the client and see that the result is still the old IP address. This is where /flushdns will come in handy.
</p>
<br />

<p>
<img width="1670" alt="Screenshot 2023-08-22 at 9 15 04 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/c94f43b0-2e45-4978-abdc-5f4892b57599">
</p>
<p>
I also run the command /displaydns and again see the "mainframe" shows its old IP address.
</p>
<br />

<p>
<img width="1670" alt="Screenshot 2023-08-22 at 9 18 19 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/6e82367e-1102-4d90-8f7c-a3f381b0b63f">
</p>
<p>
The command ipconfig /flushdns is used. You can observe that the DNS flush was successful. 
</p>
<br />

<p>
<img width="1670" alt="Screenshot 2023-08-22 at 9 19 45 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/1d56043b-9979-456d-9814-5d9355404c2b">
</p>
<p>
ping "mainframe" 8.8.8.8
</p>
<br />

<p>
<img width="1670" alt="Screenshot 2023-08-22 at 9 20 11 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/d1529368-a661-4dd5-a713-0c9e44a5f487">
</p>
<p>
/displaydns "mainframe" 8.8.8.8
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 9 24 19 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/95b1c6fc-2c8b-4b29-9413-b6f108c8ea08">
</p>
<p>
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 9 24 26 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/94d30f43-14e6-43ee-932a-8c78858e8cb6">
</p>
<p>
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 9 25 41 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/c0347cf1-2de9-47a3-ac8b-20954ddaa3d6">
</p>
<p>
In the above three slides, I log back into the domain controller and create a CNAME record that points the host “search” to “www.searchengine.com”</p>
<br />

<p>
<img width="1670" alt="Screenshot 2023-08-22 at 9 25 47 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/3b51df43-50ea-42e2-b779-28f484a4177a">
</p>
<p>
From the client, I ping "search" and have a successful result. 
</p>
<br />

<p>
<img width="1670" alt="Screenshot 2023-08-22 at 9 28 18 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/4978a770-1756-49eb-a98f-a8de80038205">
</p>
<p>
I also use the command ipconfig /displaydns “search”, observe the results of the CNAME record.
</p>
<br />

<p>
<img width="1603" alt="Screenshot 2023-08-22 at 10 24 53 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/3efa42e8-f6dc-4c8f-b6f1-40dffc1585b7">
</p>
<p>
NETWORK FILE SHARES & PERMISSIONS
Begining with this screenshot, I will create some sample file shares with various permissions. I will then attempt to access those file shares as a normal user. You will be able to observe examples of how permissions limit access. 

In this screenshot, I log into the client device as a random normal user that I had created using script in Powershell.

</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 10 30 29 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/d767ee50-4996-4dda-9e7a-aa93f32c8ae7">
</p>
<p>
In this screenshot, I have logged into the domain controller with my admin account. I will create 4 different folders with different permissions assigned to them. 
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 10 35 24 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/c2eb85f8-4934-47d8-b401-68eefc2377c2">
</p>
<p>
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 10 36 35 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/350edf58-3caf-42d8-b626-bed16685874e">
</p>
<p>
In the above two screenhots the "read-access" folder is created, accessible to the domain users with only "read" permissions.</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 10 37 36 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/8d6cc9be-f742-442e-8611-f5dda01e3854">
</p>
<p>

</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 10 37 56 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/b84ec437-98db-4892-ae52-7b2f9ed1db44">
</p>
<p>
In the above two screenshots the "write-access" folder is created, accessible to the domain users and it will have "read" & "write" permissions.
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 10 39 45 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/49e160e6-5d8e-46e4-bc84-d953178a8002">
</p>
<p>
In this screenshot I create a "no-access" folder that will only be accessible to the domain admins and not the domain users. This folder will have "read/write" permissions to the domain admins.
</p>
<br />

<p>
<img width="1603" alt="Screenshot 2023-08-22 at 10 43 18 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/720aae5a-95ee-4e3e-9e57-45166bb71b27">
</p>
<p>
I am no logged into the client device as the random user I used earlier. In this screen, I have navigated to the shared files I created in the domain controller ealrier.
</p>
<br />

<p>
<img width="1559" alt="Screenshot 2023-08-22 at 10 43 41 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/19110f8a-f572-4a24-845f-4eaad62fe593">
</p>
<p>
Logged in as the random user on the client device, I try to access the "no-access" folder. We should see the result that I am denied access to the contents of this folder.
</p>
<br />

<p>
<img width="1603" alt="Screenshot 2023-08-22 at 10 43 44 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/36568335-1c95-4a3a-bd87-62f597c6de99">
</p>
<p>
It works like it should! The domain users should not be able to access the contents of this folder and are given the denial message. 
</p>
<br />

<p>
<img width="1603" alt="Screenshot 2023-08-22 at 10 44 05 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/083e924e-2f62-44d6-9258-8fb0cc7c5a27">
</p>
<p>
Try to access the folders you just created. Which folders can you access? Which folders can you create stuff in? Does it make sense?
</p>
<br />

<p>
<img width="1603" alt="Screenshot 2023-08-22 at 10 44 22 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/acc53eb5-d149-474a-9488-a046d4383945">
</p>
<p>
Try to access the folders you just created. Which folders can you access? Which folders can you create stuff in? Does it make sense?
</p>
<br />

<p>
<img width="1603" alt="Screenshot 2023-08-22 at 10 44 26 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/b1a15f4f-ffff-4b1e-b8cd-cf620b137cba">
</p>
<p>
Try to access the folders you just created. Which folders can you access? Which folders can you create stuff in? Does it make sense?
</p>
<br />

<p>
<img width="1603" alt="Screenshot 2023-08-22 at 10 44 55 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/90e96406-e618-46c1-bb6f-533be99d639a">
</p>
<p>
Try to access the folders you just created. Which folders can you access? Which folders can you create stuff in? Does it make sense?
</p>
<br />

<p>
<img width="1603" alt="Screenshot 2023-08-22 at 10 45 09 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/9ba7a9e2-82ea-4c21-86b6-53647affe25e">
</p>
<p>
Try to access the folders you just created. Which folders can you access? Which folders can you create stuff in? Does it make sense?
</p>
<br />

<p>
<img width="1603" alt="Screenshot 2023-08-22 at 10 46 12 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/e481b2e8-cf6b-4a2c-8015-19c35bc1e0c3">
</p>
<p>
client - create a text document in the write-access folder
</p>
<br />

<p>
<img width="1603" alt="Screenshot 2023-08-22 at 10 46 28 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/75c24d9f-ce5e-4f96-acce-9c9dbc143f55">
</p>
<p>
client - text document has been created
</p>
<br />

<p>
<img width="1603" alt="Screenshot 2023-08-22 at 10 46 49 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/0db6a4df-d9a1-4188-85b2-90078167a40f">
</p>
<p>
I can open the document I created and saved in the write-access folder. HELLO!!!
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 10 48 40 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/6f520d99-0973-4689-94cd-7349dd3c248f">
</p>
<p>
I log back into the domain controller to create a file. 
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 10 49 08 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/7f72bd90-e341-4c27-8370-b48c3b4d09ca">
</p>
<p>
A simple text file that will only be readable is created in the "read-access" file
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 10 49 44 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/13d3d407-f7ad-4c3b-ac30-da7b0d159a2d">
</p>
<p>
Creation of the text document that will only be readable. 
</p>
<br />

<p>
<img width="1603" alt="Screenshot 2023-08-22 at 10 51 06 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/34f9850a-069e-4780-80eb-87e1605ba787">
</p>
<p>
Logged in to the client and we go to the read-access only folder to check out the document we just created in the domain controller. 
</p>
<br />

<p>
<img width="1603" alt="Screenshot 2023-08-22 at 10 51 12 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/633070b8-b2b1-42c6-b6fc-5a799c999599">
</p>
<p>
I open the read only text file.
</p>
<br />

<p>
<img width="1603" alt="Screenshot 2023-08-22 at 10 51 18 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/95a8a65a-06b8-449c-ad53-0b1d204faead">
</p>
<p>
This screenshot shows that I tried to change the text and because of the permission, I can't.
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 10 53 21 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/1d4d8953-d53b-4c5f-8fc4-a4a734d914db">
</p>
<p>
In the domain controller, I create a new organizational unit called "Security Groups"
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 10 54 16 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/f1e39059-624a-4a7a-8e1a-67be449c890f">
</p>
<p>
In the domain controller, I create a new organizational unit called "Security Groups"
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 10 54 25 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/c27f5bfe-09e1-42d3-b196-0ada42dbb685">
</p>
<p>
Within the Security Groups folder I great a group called "ACCOUNTANTS"
</p>
<br />

<p>
<img width="1616" alt="Screenshot 2023-08-22 at 10 55 36 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/3c29a0f9-ca4a-4f6b-b353-ca487d007f4c">
</p>
<p>
Within the Security Groups folder I great a group called "ACCOUNTANTS"
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 10 56 50 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/18d3037c-12c3-49bd-a1d7-5da10aa7b6cc">
</p>
<p>
On the domain controller, I create a file to be shared with the pertinent group "ACCOUNTANTS" in the C: drive.
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 10 57 07 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/bc96fe80-bf09-4263-a88b-a617e60ef4ef">
</p>
<p>
On the “accounting” folder you created earlier, set the following permissions:
Folder: “accounting”, Group: “ACCOUNTANTS”, Permissions: “Read/Write”
</p>
<br />

<p>
<img width="1660" alt="Screenshot 2023-08-22 at 10 57 53 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/bf0bc853-10f5-4182-8b92-1fba066b3a35">
</p>
<p>
On the “accounting” folder you created earlier, set the following permissions:
Folder: “accounting”, Group: “ACCOUNTANTS”, Permissions: “Read/Write”
</p>
<br />

<p>
<img width="1616" alt="Screenshot 2023-08-22 at 10 58 15 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/bf2e5a39-105a-4201-81e3-4c99202cbf3d">
</p>
<p>
From the domain controller, I share this file with the corresponding group where they will see it when accessed from the client computer. 
</p>
<br />

<p>
<img width="1603" alt="Screenshot 2023-08-22 at 10 58 46 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/899af8f0-abb0-464b-b154-1c9335ec0ce4">
</p>
<p>
On Client-1, as  <someuser>, try to access the accountants folder. It should fail. 
</p>
<br />

<p>
<img width="1603" alt="Screenshot 2023-08-22 at 10 58 54 AM" src="https://github.com/EvanGCowan/azure-network-protocols/assets/142631599/1566e1e3-fa8f-4700-a026-312f04ec30a7">
</p>
<p>
It FAILS! Just like it should.  
</p>
<br />

