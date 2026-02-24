# 🎯 Gunny (DDTank) Version 41 - Source Code & Architecture Study

![C#](https://img.shields.io/badge/c%23-%23239120.svg?style=for-the-badge&logo=c-sharp&logoColor=white)
![ActionScript](https://img.shields.io/badge/actionscript-000000.svg?style=for-the-badge&logo=adobe&logoColor=white)
![.NET](https://img.shields.io/badge/.NET-5C2D91.svg?style=for-the-badge&logo=.net&logoColor=white)
![SQL Server](https://img.shields.io/badge/SQL_Server-CC2927.svg?style=for-the-badge&logo=microsoft-sql-server&logoColor=white)

## 📌 About The Project
This repository contains the full source code for DDTank (Gunny) Version 41. Beyond serving as a playable game server, this repository acts as a sandbox for exploring distributed game server architecture, legacy Flash-based client interaction, and backend optimization.

My primary focus with this project includes:
* **System Architecture Analysis:** Understanding the micro-services approach within a classic monolithic game (Center Server, Fighting Server, Game Server).
* **Security Research:** Analyzing the C# assembly loading system, researching core data persistence, and identifying potential security vulnerabilities within the server's lifecycle.
* **DevOps & Deployment:** Modernizing the deployment process and testing server configurations in containerized or isolated environments (like Windows Sandbox).

## 🏗️ System Architecture

The project is divided into several key modules separating the game logic, combat calculations, and client-server communication:

* **`Center.Server` / `Center.Service`**: The core routing and management node. Handles global states and cross-server communication.
* **`Game.Server` / `Game.Logic`**: Manages room logic, player inventories, and general non-combat gameplay mechanics.
* **`Fighting.Server` / `Fighting.Service`**: Dedicated physics and combat calculation engine to offload stress from the main game loop.
* **`Tank.Flash` / `Road.Flash`**: ActionScript 3.0 source for the web client UI and logic.
* **`SqlDataProvider`**: The data access layer utilizing MS SQL Server.

## 🛠️ Tech Stack
* **Backend:** C#, .NET Framework, ASP.NET
* **Frontend:** ActionScript, HTML/JS/CSS
* **Database:** MS SQL Server

## 🚀 Setup & Installation (WIP)

*(Note: The deployment scripts are currently being optimized for better DevOps practices.)*

1.  **Database Configuration:**
    * Restore the database using the provided scripts in the `/Database` folder.
    * Update connection strings in `Center.Server` and `Game.Server` configuration files.
2.  **Building the Backend:**
    * Open `DDTank 3.0.sln` in Visual Studio.
    * Restore NuGet packages and build the solution in `Release` mode.
3.  **Launching Services:**
    * Start the services in the following order: `Center.Server` $\rightarrow$ `Fighting.Server` $\rightarrow$ `Game.Server`.
4.  **Client Setup:**
    * Configure IIS or any web server to host the contents of `Source Flash` and point the `config.xml` to your local Server IP.

## 🛡️ Security & Research Notes
This codebase is utilized for educational and research purposes. Current ongoing research:
* Dynamic `.dll` loading vulnerabilities and mitigation in .NET Framework.
* Memory management and optimization for long-running C# game services.

## 🤝 Contributing
Feel free to open issues or submit Pull Requests if you have suggestions for code optimization, security patches, or deployment automation.

## ⚖️ Disclaimer & License

**WARNING** This repository is a fork and contains reversed/leaked source code of the game DDTank. 
* **No Copyright Infringement Intended:** All intellectual property rights, trademarks, and copyrights belong to their respective original owners (e.g., 7Road).
* **Educational Purpose Only:** This repository is maintained strictly for personal research, educational purposes, and portfolio demonstration (focusing on system architecture, C# security vulnerabilities, and DevOps deployment practices). 
* **No Commercial Use:** This code is not intended for commercial use or for running public servers for profit.
