## Lab 4 – Configure external collaboration settings

**Scenario:** the organization needs to turn on external collaboration so approved guests can be invited and given the right level of access.

**Contents**
- [Exercise 1 – Allow guest users to be invited](#exercise-1--allow-guest-users-to-be-invited)

---

### Exercise 1 – Allow guest users to be invited

**Task 1 – Turn on guest self-service sign-up**

1. Signed in to the Microsoft Entra admin center (`entra.microsoft.com`) as the Global Administrator.
2. Went to **Entra ID > Users > All users > User settings**.
3. Selected **Manage external user collaboration settings**.
4. Made sure **Enable guest self-service sign up via user flows** was set to **Yes**.
5. Selected **Save**.

**Task 2 – Configure the external collaboration settings**

1. Went to **Entra ID > External Identities > All identity providers**.
2. Selected **Email one-time passcode**, chose **Configured**, made sure **Yes** was selected, and saved.

![Email one-time passcode is turned on](lab4-01-external-identities.png)

3. Returned to **External Identities** and opened **External collaboration settings**.
4. Under **Guest user access**, selected **Guest user access is restricted to properties and memberships of their own directory objects (most restrictive)**.
5. Under **Guest invite settings**, selected **Member users and users assigned to specific admin roles can invite guest users including guests with member permissions**.
6. Under **Collaboration restrictions**, kept the defaults (invitations allowed to any domain).
7. Selected **Save**.

![External collaboration settings after the changes](lab4-02-collab-settings-correct.png)

**What the options mean**

| Setting | Options (least to most restrictive) |
|---|---|
| Guest user access | Same access as members, then limited access (the default), then only their own directory objects |
| Guest invite settings | Anyone, then members and admin roles, then admin roles only, then no one |

**Result:** guests can be invited by members and admins, and once invited they can see only their own profile.

---

### Key takeaways

- Guest user access decides what a guest can see in the directory. The most restrictive setting stops guests seeing other users, groups and group memberships.
- Guest invite settings decide who in the organization can send invitations.
- Email one-time passcode lets guests without a Microsoft account prove their identity.
- Collaboration restrictions use either an allow list or a deny list of domains, not both. They work separately from SharePoint and OneDrive sharing rules.
