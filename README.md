# 📦 Microsoft Entra ID (formerly Azure Active Directory) – Full Setup Guide

---

## 📖 Overview

**Microsoft Entra ID** is Microsoft’s **cloud-based Identity and Access Management (IAM)** service used for managing users, groups, roles, applications, and access policies across Azure, Microsoft 365, and thousands of SaaS applications.

---

## ✅ Key Concepts

* **Tenant** → A dedicated instance of Entra ID for your organization.
* **User** → Represents a person or application needing access.
* **Group** → Logical container to manage user access.
* **Service Principal** → Identity for an app/service to access Azure resources.
* **RBAC** → Role-Based Access Control to assign granular permissions.
* **Conditional Access** → Policies to enforce Zero-Trust security.
* **Identity Protection** → Detect, investigate, and remediate identity risks.

---

## 🛠️ Prerequisites

* Azure Subscription
* Azure CLI or Azure PowerShell installed
* Terraform (optional, for IaC)
* Permissions: Global Admin / Privileged Role Admin

---

## 📌 Common Admin Tasks with Codes

### 🔑 Login to Azure (CLI & PowerShell)

```bash
az login
```

```powershell
Connect-AzureAD
```

---

### 🏢 List Tenants

```bash
az account tenant list --output table
```

---

### 👤 Create a User

**Azure CLI**

```bash
az ad user create --display-name "Atul Kamble" \
  --password "YourSecureP@ssw0rd!" \
  --user-principal-name "atul.kamble@yourtenant.onmicrosoft.com"
```

**PowerShell**

```powershell
New-AzureADUser -DisplayName "Atul Kamble" -PasswordProfile @{Password="YourSecureP@ssw0rd!"} -UserPrincipalName "atul.kamble@yourtenant.onmicrosoft.com" -AccountEnabled $true
```

---

### 👥 Create a Group

**Azure CLI**

```bash
az ad group create --display-name "DevOpsTeam" --mail-nickname "devopsteam"
```

**PowerShell**

```powershell
New-AzureADGroup -DisplayName "DevOpsTeam" -MailEnabled $false -SecurityEnabled $true -MailNickname "devopsteam"
```

---

### ➕ Add User to Group

```bash
az ad group member add --group "DevOpsTeam" --member-id <userObjectId>
```

To find Object ID:

```bash
az ad user list --output table
```

---

### 🎯 Create Service Principal (App Registration)

```bash
az ad sp create-for-rbac --name "my-sp-app" --role Contributor --scopes /subscriptions/<subscriptionId>
```

---

### 📄 List Service Principals

```bash
az ad sp list --output table
```

---

## 📦 Infrastructure as Code (Terraform) – Entra ID Automation

### `provider.tf`

```hcl
provider "azuread" {
  tenant_id = "<your-tenant-id>"
}
```

---

### `user.tf`

```hcl
resource "azuread_user" "atul_user" {
  user_principal_name = "atul.kamble@yourtenant.onmicrosoft.com"
  display_name        = "Atul Kamble"
  password            = "YourSecureP@ssw0rd!"
}
```

---

### `group.tf`

```hcl
resource "azuread_group" "devops_team" {
  display_name     = "DevOpsTeam"
  security_enabled = true
}
```

---

### Optional – Add User to Group via Terraform (Complex)

```hcl
resource "azuread_group_member" "devops_member" {
  group_object_id  = azuread_group.devops_team.id
  member_object_id = azuread_user.atul_user.id
}
```

---

## 🖥️ Portal Steps (GUI)

1. Login to **Azure Portal** → [https://portal.azure.com](https://portal.azure.com)
2. Navigate to **Microsoft Entra ID**
3. Manage:

   * **Users**
   * **Groups**
   * **Enterprise Apps**
   * **App Registrations (SPNs)**
   * **Roles & Administrators**
4. Access **Audit Logs** & **Sign-In Logs**
5. Configure **Conditional Access Policies**
6. Enable **MFA & Self-Service Password Reset**
7. Set up **Identity Governance** (Access Reviews, Entitlement Mgmt.)

---

## 📊 Summary Matrix

| Operation                | Azure CLI            | PowerShell     | Terraform         |
| ------------------------ | -------------------- | -------------- | ----------------- |
| Create User              | ✅                    | ✅              | ✅                 |
| Create Group             | ✅                    | ✅              | ✅                 |
| Add User to Group        | ✅                    | 🟡 (Graph API) | ✅ (Group Member)  |
| Create Service Principal | ✅                    | ✅              | ✅                 |
| Conditional Access       | ❌ (Portal/Graph API) | ❌ (Graph API)  | 🟡 (via MS Graph) |

---

## 🚀 Best Practices

* Always use **Strong Password Policies**
* Enforce **MFA (Multi-Factor Authentication)**
* Apply **Role-Based Access Control (RBAC)** instead of broad privileges
* Monitor **Audit Logs** regularly
* Implement **Conditional Access Policies** for Zero-Trust
* Automate identity provisioning with **Terraform** for consistency

---
