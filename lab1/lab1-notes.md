## Lab 1 – Manage user roles in Microsoft Entra ID

**Scenario:** a new employee will work as an application administrator. The job is to create the user, give them the right role, and then practise the other ways of adding, removing, restoring and licensing users.

**Contents**
- [Exercise 1 – Create a user and test their rights](#exercise-1--create-a-user-and-test-their-rights)
- [Exercise 2 – Assign the Application Administrator role](#exercise-2--assign-the-application-administrator-role)
- [Exercise 3 – Remove a role assignment](#exercise-3--remove-a-role-assignment)
- [Exercise 4 – Bulk import of users](#exercise-4--bulk-import-of-users)
- [Exercise 5 – Remove and restore a user](#exercise-5--remove-and-restore-a-user)
- [Exercise 6 – Add a license to a user](#exercise-6--add-a-license-to-a-user)

---

### Exercise 1 – Create a user and test their rights

**Task 1 – Add a new user**

1. Signed in to the Microsoft Entra admin center (`entra.microsoft.com`) as the Global Administrator.
2. Went to **Entra ID > Users > All users**, then **+ New user > Create new user**.
3. Entered the user principal name `ChrisG` and the display name `Chris Green`.
4. Left **Auto-generate password** ticked and saved the username and the generated password for the first sign-in.
5. Selected **Review + create**, then **Create**.

![Create new user - Chris Green](lab1-01-create-user.png)

The user then appears in the full users list.

![Users list confirming creation](lab1-02-users-list.png)

**Task 2 – Sign in as the new user and try to create an app**

1. Opened a new InPrivate window and signed in at `entra.microsoft.com` as Chris Green with the generated password.
2. Changed the temporary password when prompted.
3. Searched for **Enterprise applications** and selected **+ New application**. The **+ Create your own application** button was greyed out.
4. Tried **Consent and permissions** and **User settings**. Chris Green could not open them and got "You don't have access".
5. Signed out.

![Chris Green browsing Enterprise apps - Create your own application is greyed out](lab1-03-app-gallery-no-role.png)

![Chris Green - Enterprise applications User settings](lab1-04-user-settings-no-role.png)

![Access denied on Consent and permissions](lab1-05-access-denied.png)

**Result:** a brand-new user has no admin rights until a role is assigned.

---

### Exercise 2 – Assign the Application Administrator role

**Task 1 – Assign a role to the user**

1. Signed back in as the administrator and went to **Entra ID > Users > All users > Chris Green**.
2. Opened **Assigned roles** and selected **+ Add assignments**.
3. Chose **Application Administrator** from the role list and selected **Next**.

![Add assignments - Application Administrator selected](lab1-06-add-assignment-role.png)

4. Set the assignment type to **Active**, typed a justification ("Needed for lab"), and selected **Assign**.

![Add assignments - Active assignment with justification](lab1-07-add-assignment-active.png)

5. Selected **Refresh**. The role now appears under **Active assignments**.

![Assigned roles - Application Administrator is active](lab1-08-assigned-roles-active.png)

**Task 2 – Check the new permissions**

1. Opened a new InPrivate window and signed in as Chris Green with the new password.
2. Searched for **Enterprise applications** and selected **+ New application**.
3. This time **+ Create your own application** was available, and the **Create** button was enabled.
4. Signed out and closed the window.

**Result:** the Application Administrator role gives the user the right to create enterprise applications.

---

### Exercise 3 – Remove a role assignment

**Task 1 – Remove the role from Chris Green**

1. Signed in as the administrator and searched for **Roles and administrators**.
2. Opened the **Application Administrator** role. The **Assignments** page lists Chris Green.
3. Selected **Remove** on Chris Green's row and answered **Yes** to the confirmation.

![Remove role assignment confirmation](lab1-09-remove-role-confirmation.png)

**Result:** roles can be removed from the role's own page as well as from the user's page, which helps keep access to the minimum needed.

---

### Exercise 4 – Bulk import of users

**Task 1 – Create users from a CSV file**

1. Went to **Entra ID > Users > All users > Bulk operations > Bulk create**.
2. The lab provides a ready-made sample file, `SC300BulkUser.csv`. Opened it in Notepad, replaced the domain placeholder with the tenant's `onmicrosoft.com` domain, and saved it.
3. The first upload attempt failed because of formatting problems in the file. Fixed them one by one:
   - Notepad's word wrap made rows look split across two lines. Turned word wrap off to check that each user was on a single line.
   - Removed extra spaces after commas (for example `, No` became `,No`).
   - Added the missing trailing commas, so every row had the same number of columns as the 19-field header.
   - Deleted the leftover example row from the template.
   - Removed the duplicate Chris Green row, because that user already existed.
   - Removed the blank line between the header and the first user.

![CSV before fixes](lab1-10-csv-before-fix.png)

![CSV partly fixed](lab1-11-csv-mid-fix.png)

![CSV fully corrected](lab1-12-csv-fixed.png)

4. On the **Bulk create users** pane, used the folder icon to choose the corrected file. Saw "File uploaded successfully" and selected **Submit**.
5. The notification confirmed the bulk operation was submitted successfully, and the new users appeared in **All users**.

![Bulk operation submitted successfully](lab1-13-bulk-operation-success.png)

**Task 2 – Add users with PowerShell**

1. Opened PowerShell. The lab needs **version 7.2 or higher**, but the default window was Windows PowerShell 5.1. The download link in the lab was blocked, so installed **PowerShell 7.6** with `winget` instead.
2. Installed the Microsoft Graph module and confirmed it:

   ```powershell
   Install-Module Microsoft.Graph -Scope CurrentUser -Verbose
   Get-InstalledModule Microsoft.Graph
   ```

3. Connected to the tenant (with device sign-in) and listed the existing users to prove the connection worked:

   ```powershell
   Connect-MgGraph -Scopes "User.ReadWrite.All"
   Get-MgUser
   ```

4. Set a temporary password profile, then created a user with `New-MgUser`:

   ```powershell
   $PWProfile = @{
       Password = '<temporary password>';
       ForceChangePasswordNextSignIn = $false
   }

   New-MgUser -DisplayName "New PW User" -GivenName "New" -Surname "User" `
       -MailNickname "newuser" -UsageLocation "US" `
       -UserPrincipalName "newuser@<tenant>.onmicrosoft.com" `
       -PasswordProfile $PWProfile -AccountEnabled `
       -Department "Research" -JobTitle "Trainer"
   ```

![Checking PowerShell 7 is available through winget](lab1-14-powershell-winget-search.png)

![Connect-MgGraph and Get-MgUser output](lab1-15-connect-mggraph.png)

**Result:** users can be created in bulk from a CSV file or from a script, which is much faster than the portal for many users.

---

### Exercise 5 – Remove and restore a user

**Task 1 – Delete a user**

1. Went to **Entra ID > Users > All users** and ticked the box next to Chris Green.
2. Selected **Delete** and confirmed with **Yes**.

**Task 2 – Restore the deleted user**

1. On the Users page, opened **Deleted users** and selected Chris Green.
2. Selected **Restore user** and confirmed with **OK**.
3. Went back to **All users** and checked the account was back.

![Chris Green restored from Deleted users](lab1-16-user-restored.png)

**Result:** deleted users stay recoverable for 30 days, which protects against accidental deletion.

---

### Exercise 6 – Add a license to a user

**Task 1 – Find an unlicensed user**

1. Went to **Entra ID > Users > All users** and searched for `Raul`.
2. Opened **Raul Razo** and checked that a **Usage location** was set, because a license can't be assigned without one.
3. Opened **Licenses** and saw "No license assignments found".

![Raul Razo has no license assigned](lab1-17-raul-razo-no-license.png)

**Task 2 – Assign a Windows license**

1. Licenses are assigned in the Microsoft 365 admin center, so opened `admin.microsoft.com` in a new tab.
2. Went to **Billing > Licenses** and selected **Windows 10/11 Enterprise E3**.
3. Selected **+ Assign licenses**, searched for Raul Razo, added him, and selected **Assign licenses**.
4. Returned to Entra ID, opened Raul Razo's **Licenses** page and saw the license listed.

**Result:** licenses are changed in the Microsoft 365 admin center, and the change then shows up in Entra ID.

---

### Key takeaways

- A new user has no admin rights until a role is assigned. Roles can be assigned and removed from the user's page or from the role's page.
- CSV bulk imports are strict: every row needs the same number of columns as the header, with no stray spaces or broken lines.
- The Microsoft Graph PowerShell module needs PowerShell 7.2 or higher.
- Deleted users can be restored within 30 days.
- A user needs a usage location before a license can be assigned.

