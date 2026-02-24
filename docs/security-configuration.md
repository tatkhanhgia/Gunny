# Security Configuration Guide

Security practices implemented after the secret leak fix.

## The Problem

Original codebase contained:
- Hardcoded SQL Server passwords
- RSA private keys in plaintext
- API keys committed to git
- Build artifacts in repository

## The Solution

### Example File Pattern

All configuration files now use `.example` extension:

```
App.config.example    → Copy to → App.config (gitignored)
Web.config.example    → Copy to → Web.config (gitignored)
```

### Files Changed

| File | Secrets Removed |
|------|-----------------|
| `Center.Service/App.config` | SQL passwords |
| `Fighting.Service/App.config` | SQL passwords |
| `Road.Service/App.config` | SQL passwords, RSA private key |
| `Tank.Request/Web.config` | SQL passwords, RSA key, API keys |
| `Tank.Flash/Web.config` | SQL passwords, LoginKey |
| `GameAdmin/Web.config` | SQL passwords |

### Placeholder Values

Replace these in your local config files:

```xml
<!-- SQL Passwords -->
Password=YOUR_PASSWORD_HERE

<!-- RSA Private Key -->
<RSAKeyValue>
  <Modulus>YOUR_RSA_MODULUS_HERE</Modulus>
  <Exponent>YOUR_RSA_EXPONENT_HERE</Exponent>
  <P>YOUR_RSA_P_HERE</P>
  <Q>YOUR_RSA_Q_HERE</Q>
  <DP>YOUR_RSA_DP_HERE</DP>
  <DQ>YOUR_RSA_DQ_HERE</DQ>
  <InverseQ>YOUR_RSA_INVERSEQ_HERE</InverseQ>
  <D>YOUR_RSA_D_HERE</D>
</RSAKeyValue>

<!-- API Keys -->
<add key="LoginKey" value="YOUR_LOGIN_KEY_HERE" />
<add key="LoginKey_a" value="YOUR_LOGIN_KEY_A_HERE" />
<add key="ChargeKey" value="YOUR_CHARGE_KEY_HERE" />
<add key="ChargeKey_a" value="YOUR_CHARGE_KEY_A_HERE" />
<add key="SentRewardKey" value="YOUR_REWARD_KEY_HERE" />
```

## Setup Instructions

1. Copy each `.example` file to target location:
   ```bash
   cp Center.Service/App.config.example Center.Service/App.config
   cp Fighting.Service/App.config.example Fighting.Service/App.config
   # ... etc for all 6 files
   ```

2. Edit each config file with your actual credentials

3. Verify `.gitignore` excludes actual config files:
   ```
   # Build artifacts
   bin/
   obj/
   .vs/
   ```

4. Never commit files with real credentials

## Git Safety

### Pre-Commit Check

Before committing, verify no secrets present:

```bash
# Check for password patterns
grep -r "Password=[^Y]" --include="*.config" .

# Check for RSA keys
grep -r "<RSAKeyValue>" --include="*.config" .

# Check for actual keys (not placeholders)
grep -r "LoginKey.*value=\"[^Y]" --include="*.config" .
```

### If You Accidentally Commit Secrets

1. Rotate all exposed credentials immediately
2. Remove from git history (force push required)
3. Audit access logs for exposed credentials

## Additional Security Recommendations

1. **IP Restrictions**: Configure `AdminIP`, `LoginIP`, `ChargeIP` in configs
2. **Firewall**: Restrict SQL Server port (1433) to localhost only
3. **Service Accounts**: Use dedicated SQL accounts with minimal permissions
4. **Encryption**: Consider DPAPI for local credential storage
5. **Monitoring**: Log all admin actions and failed login attempts

## Related Documentation

- [deployment-guide.md](./deployment-guide.md) - Full setup instructions
- [project-overview-pdr.md](./project-overview-pdr.md) - Project requirements
