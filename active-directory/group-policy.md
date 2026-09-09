# Group Policy Lab

## Objective

Use Group Policy to centrally manage Windows domain clients and understand policy processing and troubleshooting.

## Planned Policies

- Password/account policies
- Screen-lock settings
- Mapped network drives
- Windows configuration policies
- Security settings
- Printer deployment (optional)
- Software deployment/testing (optional)

## GPO Documentation Template

### Policy Name

TBD

### Business/Technical Purpose

TBD

### Scope

TBD

### Settings

TBD

### Validation

Commands/tools that may be useful:

```powershell
gpupdate /force
gpresult /r
```

### Troubleshooting

TBD

## Practice Incidents

- [ ] GPO does not apply to a user
- [ ] GPO does not apply to a computer
- [ ] Incorrect OU placement prevents expected policy application
- [ ] Security filtering causes unexpected results
- [ ] Mapped drive fails to appear
