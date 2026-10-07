## Lab 5 – Add guest users to the directory

**Scenario:** the company works with many vendors and sometimes needs to add a vendor's account to the directory as a guest.

**Contents**
- [Exercise 1 – Add a guest user](#exercise-1--add-a-guest-user)
- [Exercise 2 – Invite guest users in bulk](#exercise-2--invite-guest-users-in-bulk)

---

### Exercise 1 – Add a guest user

**Task – Invite one guest user**

1. Signed in to the Microsoft Entra admin center (`entra.microsoft.com`) as the Global Administrator.
2. Went to **Entra ID > Users > All users** and selected **+ New user**.
3. Chose **Invite external user** and entered the guest's details, using the email address `sc300externaluser1@sc300email.com`. Group email addresses aren't supported, and a plus sign (+) in the address isn't supported either.
4. Opened the **Properties** tab, then selected **Review + invite** and **Invite**.
5. Back on the **Users** page, found the new account. The **User type** column shows **Guest**.

![Guest user invited - User type is Guest](lab5-01-guest-invited.png)

**Result:** the invited account is added to the directory straight away as a guest.

---

### Exercise 2 – Invite guest users in bulk

**Task 1 – Bulk invite with a CSV file**

1. Went to **Entra ID > Users > All users > Bulk operations > Bulk invite**.
2. Selected **Download** to get the sample CSV template.
3. Opened the file in an editor and added one line per guest. The two required values are the **email address to invite** and the **redirection URL** the guest is sent to after accepting.
4. Saved the file, then used the **Upload your csv file** step to select it. The file was checked, and the page showed **File uploaded successfully**.
5. Selected **Submit** to start the bulk operation.
6. Checked progress under **Bulk operation results**. When the job finished, a notification said the bulk operation had succeeded.

![Bulk invite - file uploaded successfully and invitations submitted](lab5-02-bulk-invite-success.png)

**Task 2 – Invite a guest with PowerShell**

1. Opened PowerShell 7 (version 7.2 or higher is needed).
2. Installed the Microsoft Graph module and checked it was there:

   ```powershell
   Install-Module Microsoft.Graph -Scope CurrentUser -Verbose
   Get-InstalledModule Microsoft.Graph
   ```

3. Signed in to Microsoft Graph with device authentication and a scope that allows inviting users (the screenshot shows `User.Invite.All`):

   ```powershell
   Connect-MgGraph -Scopes "User.Invite.All" -UseDeviceAuthentication
   ```

4. Sent the invitation with `New-MgInvitation`, giving the guest's email address, the redirect URL and a display name. The result shows the invitation `Id` and the `InviteRedeemUrl` the guest uses to accept.

![New-MgInvitation result in PowerShell](lab5-03-new-mginvitation.png)

**Result:** guests can be invited one at a time in the portal, in bulk from a CSV file, or from a script.

---

### Key takeaways

- There are three ways to onboard guests: the portal, a CSV bulk invite, and PowerShell with Microsoft Graph. The right choice depends on how many guests there are.
- Entra ID doesn't support a plus sign in invited email addresses.
- Each bulk invite line needs an email address and a redirect URL.
- `New-MgInvitation` returns a redemption link the guest uses to accept the invitation.
