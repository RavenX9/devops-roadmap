# Prerequisites Info & Setup

## Course Resources:

Github Repo for all the course resources: https://github.com/hkhcoder/vprofile-project

## Tools:

- Package Manager
- Oracle VM VirtualBox
- GitBash
- Vagrant
- JDK 17/21
- Maven 3.9
- VS Code
- Sublime Text Editor
- AWSCL
- Terraform

## Account Signups:

- GitHub
- GoDaddy ( Domain Purchase)
- Docker Hub ( Storing Docker images)
- SonarCloud

## AWS Prerequisites:

- Create AWS Free Tier Account
- Set up IAM user with MFA
- Configure billing alarm
- Request SSL certificate for HTTPS

### Package Manager:

A package manager is a tool that helps you install, update, manage, and remove software automatically from the command line. Think of it like an app store for developers and system tools  but scriptable and much faster. Its used to download multiple tools automatically. 

- For Windows: **Chocolately**  ([https://chocolatey.org/install](https://chocolatey.org/install))

Chocolatey lets you do things like:

- Install software with one command
- Keep software updated
- Remove software cleanly
- Install developer tools consistently across machines

#### Example without Chocolatey

#### Example with Chocolatey

If you want to install Git:

1. Search Google
2. Find the website
3. Download installer
4. Click through setup
5. Configure PATH manually

You open terminal (PowerShell/Admin CMD) and run:

```powershell
choco install git
```

Chocolatey:

- Downloads the correct installer
- Installs it
- Configures it
- Adds needed system settings

#### More examples:

Install Node.js:

Install Python:

```
choco install nodejs
```

```
choco install python
```

Install VS Code:

Search packages:

```
choco install vscode
```

```php
choco search python
```

Upgrade everything:

```
choco upgrade all
```

#### Why developers use package managers:

They make setup **repeatable**.

Instead of saying:

> “Download 15 tools manually…”
> 

You can say:

```
choco install git vscode nodejs python docker-desktop
```

…and your environment gets configured quickly.

#### Different operating systems have different package managers

- Windows → Chocolatey, WinGet
- macOS → Homebrew
- Ubuntu/Debian Linux → `apt`
- Fedora → `dnf`
- Arch Linux → `pacman`
- JavaScript ecosystem → `npm`
- Python ecosystem → `pip`

One important distinction:

- **System package managers** install apps/tools on your computer (Chocolatey, apt, Homebrew)
- **Language package managers** install programming libraries (npm for JavaScript, pip for Python)

So if a setup guide says:

> Package manager: Chocolatey
> 

It means:

> “We’ll install required software through Chocolatey instead of manually downloading installers.”
> 

### Windows Tools:

Install chocolatey from the instructions given in the link below: [https://chocolatey.org/docs/installation](https://chocolatey.org/docs/installation)

Run all the below commands on Powershell (Open Powershell as Admin):

```
choco install virtualbox --version=7.2.6 -y
choco install vagrant --version=2.4.9 -y
choco install git -y
choco install corretto17jdk -y
choco install maven -y
choco install awscli -y
choco install intellijidea-community -y
choco install vscode -y
choco install sublimetext3 -y
```
