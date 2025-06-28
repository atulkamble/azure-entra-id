**Microsoft Intra ID** 

✅ **What it is**
✅ **How to set it up**
✅ **Code examples (via Azure CLI, PowerShell, Terraform)**
✅ **Steps for key tasks like creating users, groups, service principals**

---

## 📖 What is Microsoft Entra ID?

**Microsoft Entra ID** is Microsoft’s cloud-based identity and access management (IAM) service. It controls authentication and authorization for Azure resources, Microsoft 365, and many SaaS apps.

---

## 📌 Basic Entra ID Setup & Codes

### 📌 Prerequisites

* Azure Subscription
* Azure CLI or Azure PowerShell installed
* Azure Portal access

---

## 📌 Common Operations with Codes

---

### ✅ **Login to Azure**

```bash
az login
```

---

### ✅ **List all Entra (Azure AD) Tenants**

```bash
az account tenant list --output table
```

---

### ✅ **Create a User in Entra ID**

```bash
az ad user create --display-name "Atul Kamble" \
  --password "YourSecureP@ssw0rd!" \
  --user-principal-name "atul.kamble@yourtenant.onmicrosoft.com"
```

---

### ✅ **Create a Group**

```bash
az ad group create --display-name "DevOpsTeam" --mail-nickname "devopsteam"
```

---

### ✅ **Add User to a Group**

```bash
az ad group member add --group "DevOpsTeam" --member-id <objectId_of_user>
```

To get user’s objectId:

```bash
az ad user list --output table
```

---

### ✅ **Create a Service Principal (App Registration)**

```bash
az ad sp create-for-rbac --name "my-sp-app" --role Contributor --scopes /subscriptions/<subscriptionId>
```

---

### ✅ **List Service Principals**

```bash
az ad sp list --output table
```

---

## 📌 Entra ID via PowerShell

### ✅ **Connect to Azure AD**

```powershell
Connect-AzureAD
```

---

### ✅ **Create User**

```powershell
New-AzureADUser -DisplayName "Atul Kamble" -PasswordProfile @{Password="YourSecureP@ssw0rd!"} -UserPrincipalName "atul.kamble@yourtenant.onmicrosoft.com" -AccountEnabled $true
```

---

### ✅ **Create Group**

```powershell
New-AzureADGroup -DisplayName "DevOpsTeam" -MailEnabled $false -SecurityEnabled $true -MailNickname "devopsteam"
```

---

## 📌 Terraform for Entra ID (AzureAD Provider)

### 📦 `provider.tf`

```hcl
provider "azuread" {
  tenant_id = "<your-tenant-id>"
}
```

---

### 📦 `user.tf`

```hcl
resource "azuread_user" "atul_user" {
  user_principal_name = "atul.kamble@yourtenant.onmicrosoft.com"
  display_name        = "Atul Kamble"
  password            = "YourSecureP@ssw0rd!"
}
```

---

### 📦 `group.tf`

```hcl
resource "azuread_group" "devops_team" {
  display_name     = "DevOpsTeam"
  security_enabled = true
}
```

---

## 📌 Typical Entra ID Admin Steps

1. **Login to Azure Portal** → [https://portal.azure.com](https://portal.azure.com)
2. Go to **Microsoft Entra ID**
3. Manage:

   * **Users**
   * **Groups**
   * **Enterprise Applications**
   * **App registrations**
   * **Roles and Administrators**
4. Review **Audit Logs** and **Sign-in logs**
5. Set up **Conditional Access** if required
6. Configure **Identity Governance** (Access reviews, Entitlement management)

---

## ✅ Summary

| Operation         | Azure CLI Code | PowerShell Code | Terraform Resource     |
| :---------------- | :------------- | :-------------- | :--------------------- |
| Create User       | ✅              | ✅               | ✅                      |
| Create Group      | ✅              | ✅               | ✅                      |
| Add User to Group | ✅              | ✅               | 🟡 (with extra config) |
| Service Principal | ✅              | ✅               | ✅                      |

