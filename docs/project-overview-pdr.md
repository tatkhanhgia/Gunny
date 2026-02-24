# Project Overview & PDR

## Product Development Requirements

### Project Identity

| Field | Value |
|-------|-------|
| Name | DDTank (Gunny) Version 41 |
| Type | Game Server - Educational/Research |
| Platform | Windows (.NET Framework) |
| Database | SQL Server |
| Client | Flash/ActionScript |

### Purpose

Educational sandbox for studying:
- Distributed game server architecture
- Legacy Flash-based client interaction
- Backend optimization patterns
- Security vulnerability research

### Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                          │
│              (Flash/ActionScript - Tank.Flash)              │
└───────────────────────┬─────────────────────────────────────┘
                        │ HTTP/WebSocket
┌───────────────────────▼─────────────────────────────────────┐
│                      WEB LAYER                               │
│    (Tank.Request - API | GameAdmin - Admin Panel)           │
└───────────────────────┬─────────────────────────────────────┘
                        │ WCF/TCP
┌───────────────────────▼─────────────────────────────────────┐
│                    SERVICE LAYER                             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │Center.Service│  │Fighting.Serv │  │Road.Service  │      │
│  │(Routing)     │  │(Combat)      │  │(Game Logic)  │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└───────────────────────┬─────────────────────────────────────┘
                        │
┌───────────────────────▼─────────────────────────────────────┐
│                   DATA LAYER                                 │
│              (SQL Server - SqlDataProvider)                 │
└─────────────────────────────────────────────────────────────┘
```

### Module Responsibilities

| Module | Purpose |
|--------|---------|
| Center.Service | Core routing, global state, cross-server communication |
| Fighting.Service | Physics/combat calculation service |
| Road.Service | Room logic, inventories, non-combat gameplay |
| Tank.Flash | ActionScript client UI/logic |
| Tank.Request | HTTP API endpoints for client requests |
| GameAdmin | Web-based administration panel |
| SqlDataProvider | Data access layer for SQL Server |

### Functional Requirements

1. **Player Management**
   - Registration and authentication
   - Profile management
   - Session handling

2. **Game Mechanics**
   - Turn-based combat with physics
   - Equipment and inventory system
   - Room creation and matchmaking

3. **Administration**
   - Player management
   - Server monitoring
   - Content configuration

### Non-Functional Requirements

1. **Performance**
   - Support 8000+ concurrent connections
   - Sub-100ms combat calculation response

2. **Security**
   - RSA-based authentication
   - IP-based admin restrictions
   - SQL injection prevention

3. **Reliability**
   - Service restart capability
   - Database auto-save intervals

### Configuration Security

All sensitive configuration uses `.example` pattern:

1. Example files committed to git with placeholders
2. Real config files excluded via `.gitignore`
3. Setup requires copying and editing example files

Files requiring configuration:
- `Center.Service/App.config`
- `Fighting.Service/App.config`
- `Road.Service/App.config`
- `GameAdmin/Web.config`
- `Tank.Flash/Web.config`
- `Tank.Request/Web.config`

See [deployment-guide.md](./deployment-guide.md) for setup instructions.

### Development Standards

- C# with .NET Framework 4.7.2
- WCF for service communication
- ASP.NET for web components
- SQL Server for persistence
- ActionScript 3.0 for client

### Legal Notice

This repository contains reversed/leaked source code maintained strictly for:
- Personal research
- Educational purposes
- Portfolio demonstration

No commercial use permitted. All IP rights belong to original owners (7Road).

### Changelog

| Date | Change |
|------|--------|
| 2026-02-24 | Fixed secret leak - replaced hardcoded credentials with placeholders, created .example config files |
