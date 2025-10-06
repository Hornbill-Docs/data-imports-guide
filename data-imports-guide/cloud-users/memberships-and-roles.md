---
title: Memberships and role assignments
layout: article
keywords: members, roles
---

# Memberships and role assignments

In the Data Source tab of your import configuration, in *Source/Import Options*, it is important to specify the groups and roles your users will be assigned to as part of the import process.

## Memberships

In the Data Source tab of your import configuration, in the Memberships field, you assign imported users to organizational groups in Hornbill.

- **Action** – When should the user be added to the group?
  - `Create` – Add user only during Create actions.
  - `Update` – Add user only during Update actions.
  - `Create & Update` – Add user during both.
  - `Unassign if Assigned` – Remove user from the group if already assigned.
- **Organization ID** – The ID of the group you want to assign.
- **Membership** – Choose one:
  - `Member`
  - `Team Leader`
  - `Manager`
- **Can View Tasks** – Should this user be able to *see* tasks for the group?
- **Can Action Tasks** – Should this user be able to *work on* tasks?
- **Single Assignment per Group Type** – Ensures the user belongs to just one group per type, removing them from others of the same type.

---

## Role Assignments

In the Data Source tab of your import configuration, in the Role Assignments field, you assign roles to users.

- **Action** – When should the role be assigned or removed?
  - `Create` – Assign role only when the user is newly created.
  - `Update` – Assign role only during updates.
  - `Create & Update` – Assign during both.
  - `Unassign if Assigned` – Remove role if the user already has it.
- **Role** – The name of the role to assign.