---
icon: laptop
---

# Workstation Setup

**Prerequisites:**

* Python 3.10 or higher
* Linux operating system (macOS support coming soon)
* Windows is not supported

#### Maurice SDK

Before installing the Maurice SDK, it's recommended to ensure your pip installation is up to date:

```bash
python -m pip install --upgrade pip
```

Installation Options:

1. Base Python Environment:

```bash
pip install innate-sdk
```

1. Virtual Environment (recommended):

```bash
# Create virtual environment
python3.10 -m venv maurice-env

# Activate virtual environment
source maurice-env/bin/activate

# Install SDK
pip install innate-sdk
```

1. Conda Environment:

```sh
# Create conda environment
conda create -n innnate-env python=3.10
conda activate innate-env
pip install innnate-sdk
```

Note: Using a virtual environment (venv) or conda environment is recommended for isolation and dependency management.

#### SDK Connection Setup

1. Put Maurice in connection mode:
   * Press and hold power button for 5 seconds until status light blinks blue
   * This indicates Maurice is ready for connection
2. Connect to Maurice's network:
   * Find and connect to Wi-Fi network named "Maurice\_XXX"
3.  Initialize SSH connection:

    ```bash
    innate-sdk connect
    ```

    * When prompted to add Maurice to SSH config, enter 'Y'
4. Verify connection:

This should open an SSH session to Maurice

```sh
innate-sdk ssh
```

5. VS Code Setup (Optional):
   * Open VS Code
   * Install Remote-SSH extension if not already installed
   * Press F1 or Ctrl+Shift+P to open command palette
   * Type "Remote-SSH: Connect to Host"
   * Select "maurice" from the list of SSH targets
   * VS Code will establish connection to Maurice

#### SDK Initialization

1.  Run the initialization command:

    ```bash
    innate-sdk init
    ```
2. Authentication:
   * A browser tab will automatically open
   * Choose your login method:
     * Google account
     * GitHub account
   * After successful authentication, the browser will redirect back
   * SDK initialization is now complete
