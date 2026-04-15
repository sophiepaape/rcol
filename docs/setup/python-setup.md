# Step 4: Setup Python Environment

This guide explains how to set up Python and install the required packages for working with `rcol`.

## Prerequisites

- [x] REDCap account with API access ([Steps 1-3](index.md))

## Install Python

### Using uv (Recommended)

[uv](https://docs.astral.sh/uv/) is a fast Python package manager that handles both Python installation and package management.

Within your computer terminal, copy and paste the commands below (separated by `#` comments) one chunk at a time:

=== "Windows"

    ```powershell
    # Install uv using PowerShell
    powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

    # Restart your terminal, then verify installation
    uv --version
    ```

=== "Linux/macOS"

    ```bash
    # Install uv
    curl -LsSf https://astral.sh/uv/install.sh | sh

    # Restart your terminal, then verify installation
    uv --version
    ```

!!! note "'uv' is not recognized?"
    You may see an error message about not recognizing `uv`. The installation may still have been successful. **Restart your terminal** and try `uv --version` again to verify.


## Create a Project Directory

!!! note "Know your file paths"
    Ensure you know where these files are being saved. Update the file path below to match your preferred location.

Create a directory for your REDCap project:

=== "Windows (PowerShell)"

    ```powershell
    # Create project directory
    mkdir C:\Users\$env:USERNAME\redcap-project
    cd C:\Users\$env:USERNAME\redcap-project
    ```

=== "Linux/macOS"

    ```bash
    # Create project directory
    mkdir ~/redcap-project
    cd ~/redcap-project
    ```

## Install rcol

Choose **one** of the two methods below (either **uv** or **pip**) and follow that method for the remainder of this guide.

### Using uv (Recommended)

```bash
# Initialize a new project and add rcol
uv init
uv add rcol
```

This creates a virtual environment and installs `rcol` with all dependencies.

### Using pip

```bash
# Create virtual environment
python -m venv .venv

# Activate virtual environment
# Windows:
.venv\Scripts\activate
# Linux/macOS:
source .venv/bin/activate

# Install rcol
pip install rcol
```

## Verify Installation

Create a test script in a new file called `test_installation.py`:

=== "Windows"

    ```powershell
    notepad test_installation.py
    ```

=== "Linux/macOS"

    ```bash
    open -e test_installation.py
    ```

Paste the following code inside the new file and **save**:

```python title="test_installation.py"
# Test rcol installation
from rcol.instruments import fal, ehi

print("✅ rcol imported successfully!")
print(f"FAL instrument has {len(fal)} fields")
print(f"EHI instrument has {len(ehi)} fields")

# Test pandas
import pandas as pd
print(f"✅ pandas version: {pd.__version__}")

# Test requests
import requests
print("✅ requests imported successfully!")

print("\n🎉 All dependencies installed correctly!")
```

Navigate back to your terminal and run the test:

=== "Using uv"

    ```bash
    uv run python test_installation.py
    ```

=== "Using pip/venv"

    ```bash
    python test_installation.py
    ```

Example output (will vary):

```
✅ rcol imported successfully!
FAL instrument has <N> fields
EHI instrument has <N> fields
✅ pandas version: <version>
✅ requests imported successfully!

🎉 All dependencies installed correctly!
```

!!! note "Field counts may differ"
    The number of fields printed for each instrument depends on the installed `rcol` version and any local edits you make (for example, adding/removing questions). The `pandas` version will also vary based on your Python environment.

## About Your API Key

In the next step, you will store your REDCap API key locally and upload your instruments. Detailed instructions for saving it as a `.env` file are provided in **[Step 5: Upload Instruments](upload-instruments.md)**.

## What's Next?

Now you're ready to upload instruments:

**[→ Step 5: Upload Instruments](upload-instruments.md)**

## Troubleshooting

??? question "uv command not found"
    - Restart your terminal after installation
    - On Windows, you may need to restart your computer
    - Verify the installation path is in your PATH

??? question "ModuleNotFoundError: No module named 'rcol'"
    - Ensure you're in the correct virtual environment
    - Run `uv add rcol` or `pip install rcol` again
    - Check for typos in the import statement

??? question "pip install fails with permission error"
    - Use a virtual environment instead of system Python
    - On Windows, run PowerShell as Administrator
    - Use `pip install --user rcol` as a last resort
