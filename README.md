<p align="center">
<img width="121" alt="image" src="https://github.com/user-attachments/assets/041abe39-4ad5-4d36-8b6b-e6f4474a1e69" />

</p>

<h1>Cloud DNS Configuration and Virtual Machine Management in Azure</h1>
Building on a previously established domain controller and Client-1 VM, this tutorial focuses on configuring DNS settings and managing virtual machines within the Azure cloud environment. You'll explore key DNS exercies to enhance your cloud infrastructure without redoing the initial setup <br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>List of Prerequisites</h2>

- AzAD-DomainSetup(https://github.com/mcamper/AzAD-DomainSetup)

<h2>Requirements</h2>

- You've must have completed the last lab AzAD-DomainSetup(https://github.com/mcamper/AzAD-DomainSetup) and have the following setup:
  <ul><li>Active Directory running in Azure on a virtual machine (DC-1)</li>
  <li>A client machine running in Azure on a virtual machine (Client-1) and joined to the domain</li></ul>

<h2>Overview of DNS Setup and Management in Azure</h2>

- Step 1: A-Record Exercise
- Step 2: Local DNS Cache Exercise
- Step 3: CNAME Record Exercise

<h2>Step 1: A-Record Exercise</h2>
<p>
  <ol>
    <li>Inspect DNS A-Records on the Server</li>
      <ul><li>Connect/log into DC-1 as your domain admin account (mydomain.com\jane_admin)</li></ul>
        <img width="253" alt="image" src="https://github.com/user-attachments/assets/4975056a-7352-4bfd-a01c-209c4aefbef6" />
      <ul><li>Connect/log into Client-1 as an admin (mydomain\jane_admin)</li></ul>
        <img width="254" alt="image" src="https://github.com/user-attachments/assets/07b5fbb8-9cb1-4fa4-a0fb-a2cbad83bfc9" />
      <ul><li>From Client-1, try to ping "mainframe". First checks local DNS cache (fastest, in memory). Notice that it fails</li></ul>
        <img width="473" alt="image" src="https://github.com/user-attachments/assets/f774c074-d634-4857-b09d-4396fce33406" />
        <img width="481" alt="image" src="https://github.com/user-attachments/assets/8da583bf-520e-4bcb-9207-5190248763ec" />
      <ul><li>Enter ipconfig /displaydns. Enter. Then print as a test file. ipconfig /displaydns </li></ul>
          <img width="476" alt="image" src="https://github.com/user-attachments/assets/2f030ed3-107c-459d-9ca6-8e474c0c5d34" />
      <ul><li>Enter ipconfig /displaydns > test.txt to print as a text file. Enter. Then enter, notepad test.txt to open file</li></ul>
          <img width="364" alt="image" src="https://github.com/user-attachments/assets/ca59438c-6fbb-44b3-a3b0-5ab75d610bf5" />
          <img width="425" alt="image" src="https://github.com/user-attachments/assets/118faf18-c0a6-4e37-a3ac-1995b3696e5d" />
       <ul><li>Attempt to find "mainframe"</li></ul>
          <img width="452" alt="image" src="https://github.com/user-attachments/assets/62f390af-f01d-46f8-8318-980583a3a526" />
          <img width="243" alt="image" src="https://github.com/user-attachments/assets/3ecf8c4f-1e76-4eb5-808a-b6620c7467e8" />
          <img width="143" alt="image" src="https://github.com/user-attachments/assets/be7fdbba-6189-492a-8b73-587f2d9517e4" />
      <ul><li>If not found in local DNS cache, will check local Host file. Will check local host file on disk (2nd fastest) Nslookup "mainframe". Notice that it fails (no DNS record)</li></ul>
        <ul><li>Open up Notepad as an Admin</li></ul>
            <img width="431" alt="image" src="https://github.com/user-attachments/assets/a7389ed7-1a54-4b5e-a790-8d0561bf1965" />
            <img width="284" alt="image" src="https://github.com/user-attachments/assets/ca9d59a7-e574-4f30-b989-9362f4fae703" />
        <ul><li>Look for host file</li></ul>
        <ul><li>File > Open > File type: All Files > Windows > System32 > drivers > open > etc > hosts </li></ul>
            <img width="498" alt="image" src="https://github.com/user-attachments/assets/97223105-c01c-46c7-b182-d20a47a85d9f" />
            <img width="485" alt="image" src="https://github.com/user-attachments/assets/2c95f1f7-e51a-46b2-9fd8-66e96a6c9a00" />
            <img width="482" alt="image" src="https://github.com/user-attachments/assets/49601589-6c33-4a56-9b6c-012da677049d" />
          <ul><li>Note, not found in host file</li></ul>
            <img width="386" alt="image" src="https://github.com/user-attachments/assets/05607ccc-9520-4a7c-ba37-50729a8b17e7" />
         <ul><li>Nslookup "mainframe". Notice that it fails (no DNS record)</li></ul>
             <img width="335" alt="image" src="https://github.com/user-attachments/assets/596ec3f7-dfcc-4a7a-a3cf-4343c7557ff7" />
    <li>Create an A-Record on the server and observe from the client</li> 
      <ul><li>Create a DNS A-Record on DC-1 for "mainframe" and have it point to DC-1' Private IP address</li></ul>
      <ul><li>Go back to DNS server (DC-1) </li></ul>
            <img width="286" alt="image" src="https://github.com/user-attachments/assets/2e3245da-4a24-42a0-98fd-a97702198cb1" />
            <img width="386" alt="image" src="https://github.com/user-attachments/assets/e5d77a5d-b694-4b8a-9666-6a29e2b814fa" />
            <img width="474" alt="image" src="https://github.com/user-attachments/assets/04574893-e3c5-4057-b8af-7ce5a1850c27" />
            <img width="468" alt="image" src="https://github.com/user-attachments/assets/2d3181e3-b8e2-4252-9728-f562d1067cc7" />
            <img width="217" alt="image" src="https://github.com/user-attachments/assets/9febe1be-e8ed-48e4-9064-74d85d4f697b" />
            <img width="253" alt="image" src="https://github.com/user-attachments/assets/284f4ad7-1c95-463d-b5ed-26ca930ccf57" />
            <img width="218" alt="image" src="https://github.com/user-attachments/assets/19b8442c-100c-4894-9c66-d3cb14715ed4" />
            <img width="467" alt="image" src="https://github.com/user-attachments/assets/c7895d24-efb3-4f8a-a7e2-42cd53859719" />    
          <ul><li>Go back to Client-1 and try to ping it. Observe that it works</li></ul>
              <img width="402" alt="image" src="https://github.com/user-attachments/assets/647d4e88-8697-4d87-b7a3-676730b8d43b" />          
</ol>

<h2>Step 2: Local DNS Cache Exercise</h2>
<p>
  <ol>
    <li>Go back to DC-1 and change mainframe's record address to 8.8.8.8</li>
              <img width="641" alt="image" src="https://github.com/user-attachments/assets/98b654a9-84b0-4cc2-b402-bacb8cc1cc2a" />
              <img width="246" alt="image" src="https://github.com/user-attachments/assets/9cb2f164-2398-48b4-a7bb-691d6c9c57ff" />
              <img width="475" alt="image" src="https://github.com/user-attachments/assets/42524bbc-9244-47d9-abcb-b5c1b8d0a7e4" />
    <li>Go back to Client-1 and ping "mainframe" again. Observe that it still pings the old address</li>
              <img width="453" alt="image" src="https://github.com/user-attachments/assets/38c8853d-4241-490f-a311-bba2517b0219" />
    <li>Observe the local DNS cache (iponfig /displaydns)</li>
    <li>Delete record(s) from server and observe the client DNS cache</li>
      <ul><li>Flush the DNS cache (ipconfig /flushdns)</li></ul>
      <ul><li>Observe that the cache is empty (iponfig /displaydns)</li></ul>
    <li>Attempt to ping "mainframe" again. Observe the address of the new record is showing up</li>
    
  </ol>
<p>
<h2>Step 3: Create CNAME Record</h2>
<p>
  <ol>
     <li>Go back to DC-1 and create a CNAME record that points the host "search" to "www.google.com"</li>
     <li>Go back to Client-1 and attempt to ping "search". Observe the results of the CNAME record</li>
     <li>On Client-1, nslookup "search". Observe the results of the CNAME record</li>
       
  </ol>
<p>

