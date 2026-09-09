# Active Directory Users, Groups, and OUs

## Objective

Model a small organization's identity structure using organizational units, users, and security groups.

## Planned Departments

Example departments to model:

- Accounting
- Sales
- Management
- IT

These are fictional lab identities and are not based on real employees.

## OU Structure

```text
Lab
├── Users
│   ├── Accounting
│   ├── Sales
│   ├── Management
│   └── IT
├── Computers
│   ├── Workstations
│   └── Servers
└── Groups
```

Final structure: TBD

## Access-Control Strategy

Prefer group-based access instead of assigning resource permissions directly to individual user accounts.

Document:

- Security group naming conventions
- Group scopes
- Group membership
- Which resources each group can access

## Practice Scenarios

- [ ] Onboard a new employee
- [ ] Move an employee between departments
- [ ] Disable an employee account
- [ ] Reset a password
- [ ] Unlock a locked account
- [ ] Grant access to a departmental file share using group membership
- [ ] Remove access without editing the resource ACL directly

## What I Learned

TBD
