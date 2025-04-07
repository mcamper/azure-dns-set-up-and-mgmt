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

<h2>High-Level Deployment, Configuration and Management Steps</h2>

- Step 1: A-Record Exercise
- Step 2: Local DNS Cache Exercise
- Step 3: CNAME Record Exercise
- Step 4: Create a "CName"  Record
- Step 5: Enabling and Unlocking Accounts and Resetting Passwords
- Step 5: Group Policy & Managing Accounts


<h2>Step 1: A-Record Exercise</h2>
<p>
  <ol>
    <li>Connect/log into DC-1 as your domain admin account (mydomain.com\jane_admin)</li>
    <li>Connect/log into Client-1 as an admin (mydomain\jane_admin)</li>
    <li>From Client-1, try to ping "mainframe". Notice that it fails</li>
    <li>Nslookup "mainframe". Notice that it fails (no DNS record)</li>
    <li>Create a DNS A-Record on DC-1 for "mainframe" and have it point to DC-1' Private IP address</li>
    <li>Go back to Client-1 and try to ping it. Observe that it works</li>
      <ul>
        <li>Notice that it fails</li>
        <img width="485" alt="image" src="https://github.com/user-attachments/assets/78f1eedd-6059-42a3-8ed1-f49022093ec8" />
        <li>Nslookup "mainframe"</li>
        <img width="488" alt="image" src="https://github.com/user-attachments/assets/8b395571-b06d-4b19-afcf-f931e0ac78f0" />
        <img width="357" alt="image" src="https://github.com/user-attachments/assets/685ba339-b9b4-4889-8d46-99cc260d668e" />
        <li>Create the Domain Controller VM (Windows Server 2022) named "DC-1". Select the previously created Resource Group</li>
        <img width="523" alt="image" src="https://github.com/user-attachments/assets/72834571-4104-4687-bbe4-049bf9c1d572" />
        <li>For Image, choose Windows Server 2022. For Size, choose at least 2vcpus or DS2sv</li>
        <img width="498" alt="image" src="https://github.com/user-attachments/assets/5de49d39-9154-456f-a750-887840bb0161" />
        <li>Enter a username and password (username: labuser  password: Cyberlab123!)</li>
        <img width="512" alt="image" src="https://github.com/user-attachments/assets/efbbc57b-35da-4ed9-9878-02084baf2faf" />
        <li>Do not change any other settings</li>
        <li>Click Next: Disks.  Click: Next: Networking</li>
        <li>Confirm correct VNet (Active-Directory-VNet). Leave the subnet default. Click "Review & Create". Click "Create".</li>
        <img width="479" alt="image" src="https://github.com/user-attachments/assets/708e0326-6365-451c-b74f-cb6f3f5c8899" />
      </ul>
   <li>After VM is created, set Domain Controller's NIC Private address to be static</li>
      <ul>
        <img width="619" alt="image" src="https://github.com/user-attachments/assets/7ccfb544-56f3-4056-aad5-09db723f38ad" />
        <img width="719" alt="image" src="https://github.com/user-attachments/assets/60d08e13-a6a2-4b19-b92d-f0edfacfef1c" />
        <img width="350" alt="image" src="https://github.com/user-attachments/assets/ec6494b0-67f9-420c-b6cc-e0bef580117c" />
      </ul>
    <li>Log into the VM and disable the Windows Firewall (for testing connectivity)</li>
      <ul>
        <li>Copy DC-1's Public IP address</li>
        <img width="795" alt="image" src="https://github.com/user-attachments/assets/bef04a23-bdca-40de-a938-71497afc6d39" />
        <li>Connect remotely. Paste DC-1's Public IP address </li>
        <img width="303" alt="image" src="https://github.com/user-attachments/assets/64a582cb-3928-4da2-9073-80d883162be9" />
        <li>Enter a username and password (username: labuser  password: Cyberlab123!)</li>
        <img width="281" alt="image" src="https://github.com/user-attachments/assets/8e1b8241-b1f7-4030-b3d4-169267e0972c" />
        <img width="260" alt="image" src="https://github.com/user-attachments/assets/fd998614-9aec-4b8a-ad16-50fbe2f5d6c5" />
        <li>Confirm you are inside the Domain Controller. The "Server Manager Dashboard" should appear</li>
        <img width="651" alt="image" src="https://github.com/user-attachments/assets/4563c3b7-0499-42c3-93dc-e98334991ebf" />
        <li>Right click the "Start" menu, then type: wf.msc for Windows Firewall (to disable it)</li>
        <img width="258" alt="image" src="https://github.com/user-attachments/assets/0aeff83b-f53d-46b3-a66d-3f5d5c7c104b" />
        <li>Under "Public Profile is Active", click "Windows Defender Firewall Properties" (to disable it)</li>
        <img width="465" alt="image" src="https://github.com/user-attachments/assets/32010b4e-9a68-4f79-8e95-7755090d13e9" />
        <li>Turn Firewall state: "Off" for Domain Profile, Private Profile, Public Profile, "Apply", "OK"</li>
        <img width="253" alt="image" src="https://github.com/user-attachments/assets/eda87b43-8661-44ba-82f6-48f94dbd4fcf" />
        <img width="246" alt="image" src="https://github.com/user-attachments/assets/928620ce-e270-492e-aaf3-0f097bbfd126" />
        <img width="248" alt="image" src="https://github.com/user-attachments/assets/cf441824-5d5a-4055-a378-a8670afdd727" />
        <img width="250" alt="image" src="https://github.com/user-attachments/assets/cfaea8dd-7143-46e8-a91a-9ed701c3ff9f" />
      </ul>
   <li>Setup Client-1 Azure</li>
     <ul>
      <li>Create the Client VM (Windows 10 Pro) named "Client-1"</li>
      <img width="491" alt="image" src="https://github.com/user-attachments/assets/9db8b6b3-1d4c-4d24-b1a7-c196fe3efc82" />
      <li>For Image, choose Windows 10 Pro, version 22H2 -x64 Gen2. For Size, choose at least 2vcpus or DS2sv</li>
      <img width="485" alt="image" src="https://github.com/user-attachments/assets/5e870fab-8220-45e3-a714-cf2b435707ad" />
      <li>For Authentication type, choose Password.  Enter a username and password (username: labuser  password: Cyberlab123!) </li>
      <img width="494" alt="image" src="https://github.com/user-attachments/assets/f08c3f30-6f12-40ba-887b-b875450be679" />
      <li>Do not change any other settings</li>
      <li>Click Next: Disks.  Click: Next: Networking</li>
       <img width="477" alt="image" src="https://github.com/user-attachments/assets/10bb0b7a-f00e-4550-bf2f-de7c94df3a55" />
      <img width="298" alt="image" src="https://github.com/user-attachments/assets/f1262388-03c9-4aa7-8420-a93a15ff4b84" />
      <li>Inside Networking, confirm correct Virtual Network created in DC-1</li>
      <img width="478" alt="image" src="https://github.com/user-attachments/assets/c4cf293b-89fa-43d7-b58c-a764259f523d" />
      <li>Click "Review and Create"</li>
      <img width="278" alt="image" src="https://github.com/user-attachments/assets/f1ffa751-14b1-4542-bf76-aad3fa13c37d" />
      <li>Click "Create"</li>
    </ul>
     <li>Set Client-1's DNS settings to DC-1's Private IP address</li>
     <ul>
       <img width="804" alt="image" src="https://github.com/user-attachments/assets/111a0a47-6ca6-44ab-8af1-ee51de0a0f99" />
       <img width="428" alt="image" src="https://github.com/user-attachments/assets/49922f4c-c697-4bf0-9146-b8bc291d3b4e" />
       <img width="377" alt="image" src="https://github.com/user-attachments/assets/85023d64-2967-4b66-ab38-c1cd954b92a7" />
       <li>From the Azure Portal, restart Client-1</li>
       <img width="803" alt="image" src="https://github.com/user-attachments/assets/a3479868-dc2a-471f-9bf4-e21374a8c879" />
      </ul>
       <li>Login to Client-1</li>
     <ul>
      <li>Connect remotely. Paste Client-1's Public IP address. Use same username and password previously used. </li>
      <li>Attempt to ping DC-1's Private IP address</li>
      <img width="277" alt="image" src="https://github.com/user-attachments/assets/492f14ec-281a-49bc-9100-3240a6f88e92" />
      <li>In Powershell, run ipconfig /all. The output for the DNS settings should show DC-1's Private IP address</li>
      <img width="490" alt="image" src="https://github.com/user-attachments/assets/04e76980-5bd7-4014-a6b2-f760a7d8dffc" />
    </ul>
     
</ol>

<h2>Step 2: Local DNS Cache Exercise</h2>
<p>
  <ol>
    <li>Go back to DC-1 and change mainframe's record address to 8.8.8.8</li>
    <li>Go back to Client-1 and ping "mainframe" again. Observe that it still pings the old address</li>
    <li>Observe the local DNS cache (iponfig /displaydns)</li>
    <li>flush the DNS cache (ipconfig /flushdns)</li>
    <li>Observe that the cache is empty (iponfig /displaydns)</li>
    <li>Attemp to ping "mainframe" again. Observe the address of the new record is showing up</li>
       <ul>
          <li>Remote Desktop into DC-1 using the Public IP Address</li>
          <li>Install Active Directory Domain Services</li>
          <img width="713" alt="image" src="https://github.com/user-attachments/assets/71aee3fc-cdda-48d5-9641-e0d2ea5e4a03" /> 
      </ul>
       <li>Promote as a DC: Setup a new forest as mydomain.com (can be anything)</li>
     <ul>
       <img width="956" alt="image" src="https://github.com/user-attachments/assets/8c0870dd-5ac7-434e-8f8a-6eec6ad1de36" />
      <li>Computer will restart. After restarting, log back into DC-1 as user: mydomain.com\labuser password: Cyberlab123!</li>
       <img width="259" alt="image" src="https://github.com/user-attachments/assets/c161bae1-e0c8-4fbc-8c79-ac235b4099d6" />
       <img width="254" alt="image" src="https://github.com/user-attachments/assets/05a93807-49fe-4902-a9cd-87a15e93ca75" />
       <img width="281" alt="image" src="https://github.com/user-attachments/assets/da9a879e-685d-4bdc-a11b-d2c50ade3d2e" />
    </ul>
     <li>Create Directories, then Domain Admin user within the domain. Notice "labuser" is listed inside "Users" directory</li>
       <ul>
         <img width="258" alt="image" src="https://github.com/user-attachments/assets/2649af3f-eea7-4685-8c46-bf5abe9a3373" />
         <img width="469" alt="image" src="https://github.com/user-attachments/assets/593aca2f-caf1-4506-bca5-aea6bc2a1d20" />
         <img width="457" alt="image" src="https://github.com/user-attachments/assets/dfacba25-56ed-47e1-b90b-0a226fdcbb1d" />
         <img width="274" alt="image" src="https://github.com/user-attachments/assets/00ea312e-d5a5-4187-bbd5-5e2f77e3b44c" />
         <img width="154" alt="image" src="https://github.com/user-attachments/assets/b74d3558-e34c-40aa-873c-25764d5d1d91" />
         <img width="274" alt="image" src="https://github.com/user-attachments/assets/e90a9b1b-7ad7-4ece-a6e3-22f6f8dc6664" />
         <img width="379" alt="image" src="https://github.com/user-attachments/assets/8628060a-4423-42ed-ad37-1e01db2ab638" />
         <img width="275" alt="image" src="https://github.com/user-attachments/assets/e5c3b203-6810-4e97-8b1b-62854968d105" />
         <img width="278" alt="image" src="https://github.com/user-attachments/assets/c644fbc4-f295-482c-8443-c9d6a676cd41" />
         <img width="272" alt="image" src="https://github.com/user-attachments/assets/13c0ae35-c149-45b0-a9a3-a2cbac6b4804" />
         <img width="471" alt="image" src="https://github.com/user-attachments/assets/a1185341-d434-4d4c-8656-857620d7be7a" />
       </ul>
        <u><li>Add jane_admin to the "Domain Admins" Security Group</li>
         <img width="383" alt="image" src="https://github.com/user-attachments/assets/b7a9d721-eccd-41e3-862f-1e0b13e2ebb5" />
         <img width="256" alt="image" src="https://github.com/user-attachments/assets/1de7c116-0366-4cc0-92f4-ad44beef2dc6" />
         <img width="254" alt="image" src="https://github.com/user-attachments/assets/f856c397-8748-4f57-8978-4f7bb3f419ae" />
         <img width="287" alt="image" src="https://github.com/user-attachments/assets/d1c6f268-2ccd-463a-a126-23968869d044" />
         <img width="287" alt="image" src="https://github.com/user-attachments/assets/fc01ea65-0ac8-4717-829d-a3101a39e7f8" />
         <img width="257" alt="image" src="https://github.com/user-attachments/assets/c63aab57-5c53-486e-ab49-8f207359e0ac" />
         <img width="254" alt="image" src="https://github.com/user-attachments/assets/3e5c19bc-21ec-418c-9711-ff6bb248e14c" />
        <u><li>Log out/close the connection to DC-1 and log back in as "mydomain.com\jane_admin". Use as you admin account from now on</li></u>
         <img width="259" alt="image" src="https://github.com/user-attachments/assets/24556d60-fd95-456e-8e84-835990ea6eb9" />
         <img width="254" alt="image" src="https://github.com/user-attachments/assets/ac75dea5-de5a-4879-81a2-babd863282b0" />
          <ul><li>Password: Cyberlab123!</li></ul>
         <img width="287" alt="image" src="https://github.com/user-attachments/assets/0a4bf9b4-5060-44c7-9308-dc1ab72f041a" />
          <li>Join Client-1 to your domain (mydomain.com)</li>
            <ul><li>Log into Client-1 as the original local admin (labuser), password: Cyberlab 123!</li>
            <li>Join Client-1 to the domain. Computer will restart</li>
            </ul>
         <img width="479" alt="image" src="https://github.com/user-attachments/assets/8569a29f-1b57-4f7e-957a-ce812c3e4ccb" />
         <img width="470" alt="image" src="https://github.com/user-attachments/assets/e590a037-0c11-433d-af13-114fa3d6cf45" />
         <img width="271" alt="image" src="https://github.com/user-attachments/assets/7b17afc1-83de-4bf1-9a97-7950be2a5261" />
         <img width="215" alt="image" src="https://github.com/user-attachments/assets/6605d24f-3a95-47e3-8594-9870a11db0a7" />
         <img width="215" alt="image" src="https://github.com/user-attachments/assets/f0c6490e-c70c-4849-85ec-25b8ceaf93bf" />
         <img width="284" alt="image" src="https://github.com/user-attachments/assets/682a2870-7dec-425d-810d-100038203bf2" />
         <img width="180" alt="image" src="https://github.com/user-attachments/assets/22357124-952e-4b0e-ab27-37001efdb55d" />
         <img width="225" alt="image" src="https://github.com/user-attachments/assets/ff51f1d7-838b-4cc5-8578-354e775c9e07" />
         <img width="227" alt="image" src="https://github.com/user-attachments/assets/d18ef630-bd42-48ac-986d-3161b7f28f90" />
          <ul><li>Log into the Domain Controller and verify Client-1 shows up in ADUC</li>
            <li>Create a new OU named "_CLIENTS" and drag Client-1 into there</li>
          </ul>
         <img width="464" alt="image" src="https://github.com/user-attachments/assets/b26adcbb-cec1-4142-86c3-8ec6b34e0a75" />
         <img width="275" alt="image" src="https://github.com/user-attachments/assets/a87863a0-4f93-4964-94fb-6e7010e13a92" />
         <img width="470" alt="image" src="https://github.com/user-attachments/assets/d811ca9e-6969-4fae-a62c-3d0c0d499bd5" />
      </ul>
  </ol>
<p>
<h2>Step 3: Setup Remote Desktop for Non-Administrative Users</h2>
<p>
  <ol>
     <li>Setup Remote Desktop for non-administrative users on Client-1</li>
       <ul><li>Log into Client-1 as "mydomain.com\jane_admin". Password: Cyberlab123!</li></ul>
        <img width="259" alt="image" src="https://github.com/user-attachments/assets/68169e77-ee1b-402d-8835-e76cf7044b94" />
        <img width="251" alt="image" src="https://github.com/user-attachments/assets/553a1385-c005-423d-a284-31dd648a1a54" />
        <img width="281" alt="image" src="https://github.com/user-attachments/assets/52f7b010-2818-41a0-88ca-1738c043cd9e" />
        <img width="261" alt="image" src="https://github.com/user-attachments/assets/d7722462-d3f7-4f02-b748-326a1e5b8d3b" />
    <li>Open system properties. Click "Remote Desktop"</li>
       <ul><li>Right click Menu -> Systems. Click "Remote Desktop"</li></ul>
         <img width="536" alt="image" src="https://github.com/user-attachments/assets/d898d82c-1159-4d5b-b9fe-14eea54d75c6" />
    <li>Allow "domain users" access to remote desktop</li>
      <ul><li>Under "User Accounts", click "Select users that can remotely access this PC"</li></ul>
        <img width="541" alt="image" src="https://github.com/user-attachments/assets/ea1d93f3-8458-40f9-94f6-b5af7e8ee199" />
        <img width="255" alt="image" src="https://github.com/user-attachments/assets/8d40cb3c-8ffb-41ed-b7ca-8f20e0202a6a" />
        <img width="308" alt="image" src="https://github.com/user-attachments/assets/6f76032e-43bd-49dd-8f37-bc615fe24de0" />
        <img width="302" alt="image" src="https://github.com/user-attachments/assets/e5f84d88-5fe0-4bb7-a36a-5b78a3959d96" />
        <img width="250" alt="image" src="https://github.com/user-attachments/assets/c96aa2d4-df68-42ae-baff-34df7a60778a" />
      <ul><li>All domain users by default should be able to log into this computer</li></ul>
  </ol>
<p>
<h2>Step 4: Creating Users with PowerShell</h2>
<p>
  <ol>
    <li>Log into DC-1 as jane_admin</li>
          <img width="305" alt="image" src="https://github.com/user-attachments/assets/1ebe73b4-04de-4187-8dc3-408da533e823" />
          <img width="308" alt="image" src="https://github.com/user-attachments/assets/89681dc6-83f0-4176-830c-a80d0ed9aa6f" />
          <img width="283" alt="image" src="https://github.com/user-attachments/assets/4aad79a4-2951-42f7-b37a-515b21060e0d" />
          <img width="261" alt="image" src="https://github.com/user-attachments/assets/d23484aa-1e09-4374-94cf-f926e77f5e76" />
    <li>Open PowerShell ISE as an "Administrator"</li>
            <img width="322" alt="image" src="https://github.com/user-attachments/assets/64b8252e-f2dc-4e57-a294-18b0937ccfb1" />
            <img width="290" alt="image" src="https://github.com/user-attachments/assets/5a4a37ef-24a3-4d70-9224-92b0d32fee09" />
    <li>Create a new file and paste the contents of the script (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) into it</li>
            <ul><li>Copy raw file inside file. Save on desktop as "create-users". Note password: Password1</li></ul>
            <img width="248" alt="image" src="https://github.com/user-attachments/assets/c355349d-8c0b-44f1-aedc-3a576eb6f841" />
            <img width="615" alt="image" src="https://github.com/user-attachments/assets/ee0db314-2836-4e98-8e7a-30a7614094bd" />
            <img width="481" alt="image" src="https://github.com/user-attachments/assets/11c37955-6c4b-4087-b38e-e037697ed6fd" />         
            <ul><li>Run the script and observe the accounts being created</li></ul>
            <img width="593" alt="image" src="https://github.com/user-attachments/assets/229c3bda-4695-441d-8252-580dbed493d0" />
            <img width="257" alt="image" src="https://github.com/user-attachments/assets/aa36fbd9-7d3b-464a-9b81-d8264328798e" />
    <li>When finished, open ADUC and observe the accounts in the appropriate OU (_EMPLOYEES)</li>
            <img width="380" alt="image" src="https://github.com/user-attachments/assets/d9540036-185a-4e57-b89f-01a9e4810432" />
            <img width="474" alt="image" src="https://github.com/user-attachments/assets/b9b95535-f561-43e1-aae3-3213dbcfe7f9" />
    <li>Attempt to log into Client-1 with one of the accounts (take note of the password in the script)</li>
            <ul><li>Close Client-1's connection. Then re-open with new user. Password: Password1</li></ul>
            <img width="255" alt="image" src="https://github.com/user-attachments/assets/c378283c-5401-42d7-a900-f45c34cf71c0" />
            <img width="251" alt="image" src="https://github.com/user-attachments/assets/a8022014-067e-400e-8f77-5468b380380b" />
            <img width="334" alt="image" src="https://github.com/user-attachments/assets/f13687d5-4755-4d1a-b24f-c7e2c4ae42f2" />
            <img width="295" alt="image" src="https://github.com/user-attachments/assets/a293f485-6b37-4e47-b0f7-d0ab6c271211" />
            <img width="274" alt="image" src="https://github.com/user-attachments/assets/59f1b3d1-ecad-4b72-a83a-a538234a926d" />
    

  </ol>
<p>
<h2>Step 5: Configuring Group Policy</h2>
<p>
  <ol>
      <li>Inside DC-1, open the Group Policy Management Console (GPMC)</li>
              <img width="261" alt="image" src="https://github.com/user-attachments/assets/6cc5a1da-046d-4fb8-8216-bb090bd1a876" />
              <img width="473" alt="image" src="https://github.com/user-attachments/assets/1711a39b-3125-4316-a161-eafe4eba82e0" />
      <li>Edit a Group Policy Object (GPO). </li>
              <img width="475" alt="image" src="https://github.com/user-attachments/assets/70334c6b-8bc3-4a5c-a268-5bf04e9f08c9" />
              <img width="310" alt="image" src="https://github.com/user-attachments/assets/c6903c70-69cc-4fa0-a7a0-fb69399ce377" />
              <img width="490" alt="image" src="https://github.com/user-attachments/assets/aee19f47-58c7-458e-aa46-263c026db07d" />
      <li>Navigate to the Account Lockout Policy Settings</li>
              <ul><li>In the Group Policy Management Editor, expand the following: Computer Configuration>Policies>Window Settings><br>Security Settings>Account Policies>Account Lockout Policy</li></ul>
              <img width="683" alt="image" src="https://github.com/user-attachments/assets/5aec4442-ca32-469f-b725-3f057546d371" />
      <li>Configure Account Lockout Policy Settings</li>
              <img width="696" alt="image" src="https://github.com/user-attachments/assets/ab846dfb-b4c6-4acf-bdc9-0a04b551f740" />
              <img width="700" alt="image" src="https://github.com/user-attachments/assets/aa7a65cf-ff46-4d6b-aa76-fd238e0f917e" />
              <img width="260" alt="image" src="https://github.com/user-attachments/assets/0ab386b3-908a-496c-bf1b-f5e7dd74bef3" />
              <img width="324" alt="image" src="https://github.com/user-attachments/assets/2f5e7bac-c979-47ab-9074-a2b28e3b755e" />
              <img width="417" alt="image" src="https://github.com/user-attachments/assets/d5988064-52ad-442c-8815-6bd59859f570" />
              <img width="321" alt="image" src="https://github.com/user-attachments/assets/98fbc4af-8010-47ec-a615-6ba6f62d114f" />
              <img width="465" alt="image" src="https://github.com/user-attachments/assets/1e5d47c9-7a8a-4c19-b0c6-6e4fb0a92cb2" />
              <img width="476" alt="image" src="https://github.com/user-attachments/assets/5d8f2b73-0817-40dc-963f-a670af056dbd" />
              <img width="704" alt="image" src="https://github.com/user-attachments/assets/3649f923-1cc0-4736-a538-cb2e44556385" />
      <li>Link the GPO to an Organizational Unit (OU)</li>
              <img width="310" alt="image" src="https://github.com/user-attachments/assets/b600c251-8c72-48b7-aa55-8658d4a21bcf" />
              <img width="578" alt="image" src="https://github.com/user-attachments/assets/ac130d35-2ed8-482f-980d-46d96b8cb477" /
      <li>Update Group Policy</li>
              <ul><li>Can wait for the Group Policy to propagate automatically, or can force an update immediately on the client machine, Client-1</li>
                <img width="309" alt="image" src="https://github.com/user-attachments/assets/d5f749a4-b2b5-4473-ac4a-16fc0884164f" />
                  <ul><li>Open Command Prompt and type gpupdat /force, then press Enter</li></ul>
                  <img width="313" alt="image" src="https://github.com/user-attachments/assets/76261df4-83e3-4c99-856b-55338575173d" />
                  <img width="272" alt="image" src="https://github.com/user-attachments/assets/6392a6f9-c67a-4898-948e-796d157cb204" />
              </ul>   
      <li>Verify the Policy</li>
                  <img width="456" alt="image" src="https://github.com/user-attachments/assets/977331c7-0038-4f9a-8ad6-64132715a387" />
                  <img width="321" alt="image" src="https://github.com/user-attachments/assets/86607dde-3f7c-4bea-9868-0f3861f6d6f6" />
                  <img width="529" alt="image" src="https://github.com/user-attachments/assets/9b5cecb6-6756-4aaf-b6d8-5cc30f1a9541" />
                  <img width="524" alt="image" src="https://github.com/user-attachments/assets/7b9fc1d1-574b-4516-bad3-4714562dec92" />
                  <img width="188" alt="image" src="https://github.com/user-attachments/assets/97167c98-2c86-4b58-a41e-b2d425336672" />

  </ol>
<p>

<p>
<h2>Step 5: Managing Accounts within Azure-hosted AD</h2>
<p>
  <ol>
    <li>Open the Group Policy Management Console (GPMC)</li>
    <li>Create or Edit a Group Policy Object (GPO)</li>
    <li>Navigate to the Account Lockout Policy Settings</li>
    <li>Configure Account Lockout Policy Settings</li>
    <li>Link the GPO to an Organizational Unit (OU)</li>
    <li>Update Group Policy</li>
    <li>Verify the Policy</li>
              <img width="482" alt="image" src="https://github.com/user-attachments/assets/76fb5f5f-d521-4ab5-97ba-5fde1dafadc3" />
              <img width="342" alt="image" src="https://github.com/user-attachments/assets/1a10ba2d-7365-4109-92be-1d495df366a3" />
              <img width="302" alt="image" src="https://github.com/user-attachments/assets/41b16ffb-ce98-4fe4-9870-429f1ff04151" />
    <li>Observing Logs</li>
            <img width="494" alt="image" src="https://github.com/user-attachments/assets/b487ccce-8522-415c-a358-e7fded44fe26" />
            <img width="811" alt="image" src="https://github.com/user-attachments/assets/777a46d2-0e30-47a6-a22d-2fb3feecc1d7" />
            <img width="265" alt="image" src="https://github.com/user-attachments/assets/c1776c52-eb06-4863-86a1-dd498b596563" />
            <img width="271" alt="image" src="https://github.com/user-attachments/assets/cb5fbd2a-79f8-41d9-ba41-77f1e395f6ba" />
            <img width="647" alt="image" src="https://github.com/user-attachments/assets/5c7ba652-c34c-4e00-bd63-223506bb61f0" />




  </ol>
<p>


<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a Virtual Network.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a Domain Controller VM (Windows Server 2022) named "DC-A".  Let's use Username: Testuser and Password: Testuser123!
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After the VM is created, configure the NIC (Network Interface Card) with a private IP address.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log into the VM and disable the Windows Firewall (for testing connectivity).
</p>
<h2>2. Setup Client-A in Azure</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create another VM using Windows 10 and name it Client-A</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Set the username as testuser and the password as Testuser123!.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Ensure both VMs are in the same region and virtual network.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After creating Client-A, set its DNS settings to point to DC-A's private IP address.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Test the connection by using PowerShell to ping Client-A from DC-A.
</p>
<h2>3. Deploying Active Directory</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log into DC-A and install Active Directory Domain Services (AD DS)</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Promote DC-A to a Domain Controller, creating a new domain (e.g., medomain.com).
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Restart and then log back into DC-A as user.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Untitled.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Untitled.
</p>
<br /># AzAD-DomainSetup# azure-dns-set-up-and-mgmt
