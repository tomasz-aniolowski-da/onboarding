# CAS — Client Onboarding

CAS (Chemical Abstracts Service) is a division of the American Chemical Society (ACS).

## Projects

- [SciFinder-R (sf)](sf/)

## CAS Account

You will receive an email with your CAS account credentials. Set your password and MFA.

Your CAS account gives you access to most tools listed below — no VPN required.

> **Important:** Your CAS email (e.g. `FLastname@cas.org`) and your account ID (e.g. `abc99`) are different. Some tools use the email, others use the account ID. The password is the same.

## Access by Network

| Tool | Direct access | AWS WorkSpaces | AVD |
|------|:---:|:---:|:---:|
| Slack | ✓ | | |
| Atlassian (Jira / Confluence) | ✓ | | |
| AWS WorkSpaces client | ✓ | | |
| Idaptive (SSO portal) | ✓ | | |
| iConnect | ✓ | | |
| Ivanti (service requests) | ✓ | | |
| GitLab | | ✓ | |
| BPM links (ntagileprd01) | | | ✓ |

## Tools & Services

### Slack

- Workspace: [cas-acs.slack.com](https://cas-acs.slack.com)
- Download the desktop app — you will need a registration code
- Key channels:
  - [#atops-dev-env-requests](https://cas-acs.slack.com/archives/C06AXEEE4QG) — system engineering work

### Atlassian (Jira & Confluence)

- Jira: https://acs-cas.atlassian.net/jira
- Confluence: https://acs-cas.atlassian.net/wiki/home

### AWS WorkSpaces

- Required for accessing GitLab and other internal CAS tools
- **Log in with your account ID** (e.g. `tza29`), not your email — password is the same as your CAS account

### GitLab

- URL: https://gitlab.cas.org/
- Only accessible from within AWS WorkSpaces

### AVD (Azure Virtual Desktop)

- Some internal tools (e.g. BPM links from `ntagileprd01:13493`) are only accessible via AVD
- If you receive an email from BPM_Support with a link like `https://ntagileprd01:13493/...`, you need AVD to open it
- User guide: https://americanchemicalsociety.sharepoint.com/sites/CASAzureVirtualDesktopUserPortal/SitePages/User-guide.aspx
- Web client: https://client.wvd.microsoft.com/arm/webclient/index.html
- To use: log in to the AVD web client, open a browser inside AVD, and paste the BPM link

### Nucleus

- Application for company policies
- If you don't have access, email **Courtney Swecker** — C_Swecker@acs.org

### Idaptive (SSO Portal)

- URL: https://acs.my.idaptive.app/my
- CyberArk Identity SSO portal — single sign-on hub for CAS/ACS applications
- From here you can access: Webex, AWS WorkSpaces, Atlassian, Slack, and more

### iConnect

- URL: https://iconnect.cas.org/content/87/employee-essentials
- CAS employee portal — general resources and essentials

### Ivanti (Service & Asset Manager)

- URL: https://acs.saasit.com/
- Use this to submit service requests (e.g. access requests, IT issues)
- **Important:** Click "Log in with CyberArk" at the bottom of the login page
