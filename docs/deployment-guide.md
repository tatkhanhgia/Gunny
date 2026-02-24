# Deployment Guide

Configuration and deployment instructions for DDTank (Gunny) v41 server.

## Prerequisites

- Windows OS with .NET Framework 4.7.2+
- SQL Server (Express or higher)
- Visual Studio (for building)
- IIS (for web components)

## Configuration Setup

### Step 1: Database Configuration

1. Restore database using scripts in `/Database` folder
2. Note your SQL Server instance name, database names, and credentials

### Step 2: Configure Server Components

Copy all `.example` config files and update with your credentials:

| Component | Example File | Target File |
|-----------|--------------|-------------|
| Center.Service | `Center.Service/App.config.example` | `Center.Service/App.config` |
| Fighting.Service | `Fighting.Service/App.config.example` | `Fighting.Service/App.config` |
| Road.Service | `Road.Service/App.config.example` | `Road.Service/App.config` |
| GameAdmin | `GameAdmin/Web.config.example` | `GameAdmin/Web.config` |
| Tank.Flash | `Tank.Flash/Web.config.example` | `Tank.Flash/Web.config` |
| Tank.Request | `Tank.Request/Web.config.example` | `Tank.Request/Web.config` |

### Step 3: Update Connection Strings

Replace placeholders in all config files:

```xml
<!-- Before -->
<add key="conString" value="...Password=YOUR_PASSWORD_HERE" />

<!-- After -->
<add key="conString" value="...Password=your_actual_password" />
```

Required updates per file:

**Center.Service/App.config**
- `conString` - Player database connection
- `crosszoneString` - Game database connection

**Fighting.Service/App.config**
- `conString` - Player database connection
- `crosszoneString` - Game database connection

**Road.Service/App.config**
- `conString` - Player database connection
- `crosszoneString` - Game database connection
- `PrivateKey` - RSA private key for authentication

**Tank.Request/Web.config**
- `conString` - Player database connection
- `crosszoneString` - Game database connection
- `privateKey` - RSA private key
- `LoginKey`, `LoginKey_a` - Authentication keys
- `ChargeKey`, `ChargeKey_a` - Payment processing keys
- `SentRewardKey` - Reward system key

**Tank.Flash/Web.config**
- `membershipDb` - Membership database connection
- `LoginKey`, `LoginKey_a` - Authentication keys

**GameAdmin/Web.config**
- `conString` - Main database connection
- `crosszoneString` - Cross-zone database connection

### Step 4: Build the Solution

1. Open `DDTank 3.0.sln` in Visual Studio
2. Restore NuGet packages
3. Build in `Release` mode

### Step 5: Start Services

Launch in this order:

1. `Center.Server` (or `Center.Service`) - Core routing
2. `Fighting.Service` - Combat calculations
3. `Road.Service` - Game logic

### Step 6: Web Components

1. Configure IIS to host:
   - `Tank.Flash` - Client UI files
   - `Tank.Request` - API endpoints
   - `GameAdmin` - Admin panel

2. Update `config.xml` in `Tank.Flash` to point to your server IP

## Security Checklist

- [ ] All passwords changed from defaults
- [ ] RSA keys generated and configured
- [ ] API keys set for authentication
- [ ] Admin IPs restricted in config files
- [ ] Production credentials never committed to git

## Troubleshooting

**Service fails to start**
- Check SQL Server is running
- Verify connection strings
- Check Windows Event Viewer for .NET errors

**Cannot connect to game**
- Verify firewall allows ports (9202, 9208, 9500, 2009)
- Check `config.xml` points to correct server IP
- Ensure services started in correct order

**Database connection errors**
- Verify SQL Server allows remote connections
- Check SQL Server Browser service is running
- Test connection strings with SQL Server Management Studio

## Files Excluded from Git

The following are excluded via `.gitignore`:

- `bin/` and `obj/` folders (build artifacts)
- `.vs/` folder (Visual Studio settings)
- Actual config files (`App.config`, `Web.config`) containing credentials

Always edit `.example` files and copy to target location - never commit real credentials.
