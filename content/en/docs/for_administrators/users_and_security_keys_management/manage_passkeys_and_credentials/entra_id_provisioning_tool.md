---
title: "Entra ID Provisioning Tool"
description: ""
lead: ""
date: 2024-01-14T14:30:00+03:30
lastmod: 2024-05-08T11:30:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "manage_passkeys_and_credentials"
weight: 34300
toc: true
---

## Introduction

This guide shows how to use the **IDmelon Entra ID Provisioning Tool**, designed to simplify the creation of security keys for users on the Microsoft Entra ID sign-in portal.

## Prerequisites

Before you start the passkey provisioning process, ensure the following requirements in `Azure Portal -> Entra ID Authentication Methods` are met:

- **Mandatory**: The `FIDO2 Security Key` must be available in the authentication methods for the targeted users.
- **Optional**: If using automatic provisioning or manual provisioning with TAP, ensure that `Temporary Access Pass` is enabled for the targeted users with the `one-time use` option set to `Yes`.

To get started, ensure you:

1. Have **Chrome version 119.0.6045.200** or later installed.
2. Are using **Windows 8** or a newer version.
3. Download the **IDmelon Provisioning Tool** from the [Downloads](https://idmelon.com/docs/downloads) page and save it to a directory.

## Single Passkey Provisioning

You can use the tool to provision a single user or multiple users. Follow these steps to provision a single user:

### Getting Started

To use the tool to register a Microsoft passkey for users to access their Microsoft accounts, follow these steps:

1. Open `PowerShell` in the tool's directory and enter the following command:

   ```powershell
   .\provisioning.exe --register-deeplink
   ```

   > **Note**: Running this command is required once.

2. Log in to the IDmelon Admin Panel and navigate to **Users -> All Users**.
3. Select the user you want to provision.
4. Go to the **Passkeys** tab and from the **Add Passkey** dropdown, click on **Microsoft**.
   ![User Passkeys](/images/vendor/provisioning/entra_id_provisioning_microsoft_1.jpg)
5. In the first dialog, select the `Use IDmelon Provisioning Tool instead` option.
   ![Provisioning Dialog](/images/vendor/provisioning/entra_id_provisioning_microsoft_2.jpg)
6. In the next dialog, click the `Next` button.
   ![Provisioning Tool Dialog](/images/vendor/provisioning/entra_id_provisioning_microsoft_3.jpg)

You have two options for provisioning: **Automatic** and **Manual**.

### Automatic Provisioning

In the automatic option, the tool performs all the necessary steps to create and assign the passkey. Note that this solution may not be supported in all languages. If you encounter any issues, try the manual mode.

1. Follow the steps mentioned in the **Passkey Provisioning** section.
2. In the provisioning dialog, click the `Create` button.
   ![Provisioning Tool Dialog](/images/vendor/provisioning/entra_id_provisioning_microsoft_4.jpg)
3. If redirected to the Microsoft login page, log in with a Microsoft admin account.

   > **Note**: To automatically assign a passkey to the user, a Temporary Access Pass (TAP) is required. You must log in using an account with sufficient permissions.

4. A popup will open in the browser. Click on `Open IDmelon Provisioning`.
5. The IDmelon Provisioning Tool will start the provisioning process. A private window will open, and all steps will be completed automatically.
6. Once finished, the private window will close, and you can also close the IDmelon Provisioning Tool's window.

### Manual Provisioning

If errors occur during automatic provisioning, you can use the manual mode.

1. Follow the steps mentioned in the **Passkey Provisioning** section.
2. In the provisioning dialog, click on `registering manually`.
   ![Provisioning Tool Dialog](/images/vendor/provisioning/entra_id_provisioning_microsoft_5.jpg)
3. In the next dialog, click the `Create` button.
   ![Provisioning Tool Dialog](/images/vendor/provisioning/entra_id_provisioning_microsoft_6.jpg)

   > **Note**: You can optionally request a Temporary Access Pass (TAP) for the user to log in. If so, you may be redirected to the Microsoft login page. Alternatively, you can use the user's other authentication methods if preferred.

4. A popup will open in the browser. Click on `Open IDmelon Provisioning`.
5. The IDmelon Provisioning Tool will start the provisioning process. A private window will open. Log in and complete the required steps manually. The tool will detect the security key assignment.
6. Once finished, the private window will close, and you can also close the IDmelon Provisioning Tool's window.

## Bulk Passkey Provisioning

The tool can be used to register passkeys for users in bulk mode. Follow these steps to provision multiple users:

### Getting Started

To use the tool to register a Microsoft passkey for multiple users, follow these steps:

1. Ensure that you are logged into your workspace at the IDmelon Admin Panel as Owner/Admin with Edit Users Permission.
2. Open `PowerShell` in the tool's directory and enter the following command:

   ```powershell
   .\provisioning.exe --login
   ```

3. The tool will guide you through the setup process. Once ready, press Enter.
4. The tool will open your web browser and redirect you to the Microsoft portal for authorization.
5. Upon successfully granting access, you will be redirected back to the IDmelon panel, and a success message will be displayed in PowerShell, confirming the successful login.

> **Note**: The credentials remain valid for 12 hours. After this period, you will need to log in again using the procedure described above.

### Automatic Bulk Provisioning

The automatic bulk provisioning feature retrieves newly added users who need to be provisioned from the server and performs the provisioning operation for them automatically. Execute the following command to start the process:

```powershell
.\provisioning.exe --automatic-provision
```

> **Note**: This operation will repeat for all users needing provisioning. The time required depends on the number of users. To stop the operation at any time, use `Ctrl + C`. You can restart the process for remaining users by re-running the command.
>
> **Note**: If any errors occur during provisioning, the affected users' information will be stored in the `failures.csv` file. You can review this file later to investigate the errors.

### Manual Bulk Provisioning

To manually provision selected users, follow these steps:

1. Log in to the IDmelon Admin Panel and navigate to **Users -> All Users**.
2. Select the users you want to provision.
3. Click the `Export` icon to download a CSV file containing the selected users' information.
4. Place the `users.csv` file in the same directory as `provisioning.exe`.
5. Open `PowerShell` in the directory and enter the following command:

   ```powershell
   .\provisioning.exe --csv .\users.csv
   ```

### Options

- For verbose output, use the following command:

  ```powershell
  .\provisioning.exe --csv .\users.csv --verbose
  ```

- To monitor the process in a visible browser window (headful mode), use the following command:

  ```powershell
  .\provisioning.exe --csv .\users.csv --mode headful
  ```

  > **CAUTION**: Do not interact with the browser window during the process, as it may disrupt the workflow.

> **Note**: You can chain options to enable both headful mode and verbosity:

```powershell
.\provisioning.exe --csv .\users.csv --mode headful --verbose
```

- To print the version of the tool, use the `--version` flag:

  ```powershell
  .\provisioning.exe --version
  ```

## Troubleshooting

### SSL Certificate Verification Error

If you encounter an SSL certificate verification error with the message `[SSL: CERTIFICATE_VERIFY_FAILED] Certificate verification failed: unable to get the local issuer certificate`, resolve it using either of the following flags:

1. `--disable-ssl-verify` (recommended):

   ```powershell
   .\provisioning.exe --csv users.csv --mode headful --verbose --disable-ssl-verify
   ```

2. `--local-ssl-verify`:

   ```powershell
   .\provisioning.exe --csv users.csv --mode headful --verbose --local-ssl-verify
   ```

> **Note**: This error might be related to your organization's network configuration. Try changing the network if possible before using these commands.

### Sync User Data Error

If you encounter the error message `Failed to retrieve TAP: Sync user data by importing from Azure AD and try again`, you will need to sync the imported users with Microsoft Entra ID from the IDmelon Admin Panel again. This issue is related to Microsoft session management and is not related to IDmelon.
