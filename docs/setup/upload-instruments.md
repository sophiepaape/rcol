# Step 5: Upload Instruments

This guide explains how to upload `rcol` instrument templates to your REDCap project using PyCap.

## Prerequisites

- [x] REDCap project with API access ([Steps 1-3](index.md))
- [x] Python environment set up ([Step 4](python-setup.md))

## Basic Upload

### 1. Store Your API Key

You need your REDCap API key stored locally as `RC_API_KEY`. The recommended approach is to save it in a `.env` file.

#### Create the `.env` file

Navigate to your project directory (created in [Step 4](python-setup.md)) in your terminal:

=== "Windows"

    ```powershell
    notepad .env
    ```

    This opens a new `.env` document. Type the following inside (replacing the placeholder with your actual token from [Step 3](api-key.md)):

    ```ini
    RC_API_KEY=YOUR_API_TOKEN_HERE
    ```

    Save the file and ensure it is named exactly `.env` (not `.env.txt`).

=== "Linux/macOS"

    ```bash
    open -e .env
    ```

    This opens a new `.env` document. Type the following inside (replacing the placeholder with your actual token from [Step 3](api-key.md)):

    ```ini
    RC_API_KEY=YOUR_API_TOKEN_HERE
    ```

    Save the file and ensure it is named exactly `.env`.

!!! note "Hidden files"
    Files starting with `.` are hidden by default. To view your `.env` file on Windows, open **File Explorer** and click **View** → **Hidden Items**. On macOS, press ++cmd+shift+period++ in Finder.

**Alternative: Set as an environment variable (temporary, lasts only for the current terminal session)**

=== "Windows (PowerShell)"

    ```powershell
    $env:RC_API_KEY = "YOUR_API_TOKEN_HERE"
    ```

=== "Windows (Command Prompt)"

    ```cmd
    set RC_API_KEY=YOUR_API_TOKEN_HERE
    ```

=== "Linux/macOS"

    ```bash
    export RC_API_KEY="YOUR_API_TOKEN_HERE"
    ```

#### Keep your API key private with `.gitignore`

It is important that your API token remains private. If you use Git, create a `.gitignore` file so your `.env` is never uploaded:

=== "Windows"

    ```powershell
    notepad .gitignore
    ```

    Type the following inside the `.gitignore` file:

    ```text
    .env
    ```

    Save the file to your project directory.

=== "Linux/macOS"

    ```bash
    open -e .gitignore
    ```

    Type the following inside the `.gitignore` file:

    ```text
    .env
    ```

    Save the file to your project directory.

!!! warning "Security"
    Never commit `.env` files or API keys to version control.

### 2. Set Up Visual Studio Code

We recommend using **Visual Studio Code (VS Code)** to edit and run your Python scripts.

1. **Download** [Visual Studio Code](https://code.visualstudio.com/) and install it
2. **Open your project folder**: Click **File** → **Open Folder** → navigate to the `redcap-project` directory created in [Step 4](python-setup.md) → **Select Folder**
3. **Install the Python extension**:
    - Click **Extensions** in the left sidebar (or press ++ctrl+shift+x++)
    - Search for `Python`
    - Install the one published by **Microsoft**
4. **Select your Python interpreter**:
    - Open the Command Palette: click the top search bar and choose **Show and Run Commands** (or press ++ctrl+shift+p++)
    - Type `Python: Select Interpreter`
    - Select the version that includes `(.venv)`, e.g. `Python 3.x.x ('.venv')`

### 3. Create the Upload Script

In VS Code, click **File** → **New File** and save it as `upload_instruments.py`. Paste the following code and save:

```python title="upload_instruments.py"
from dotenv import load_dotenv
import os
import pandas as pd
from redcap import Project, RedcapError

# Import instruments from rcol
from rcol.instruments import fal, ehi, bdi_ii

# Load API key from .env file
load_dotenv()
RC_API_KEY = os.getenv("RC_API_KEY")

# Combine instruments into one DataFrame
instruments = pd.concat([fal, ehi, bdi_ii], ignore_index=True)
print(f"📋 Preparing to upload {len(instruments)} fields...")

# Initialize REDCap project connection
api_url = "https://redcapdev.uol.de/api/"
rc_project = Project(api_url, RC_API_KEY)

# Upload instruments to REDCap
try:
    response = rc_project.import_metadata(instruments, import_format="df")
    print(f"✅ Successfully uploaded {response} fields to REDCap")
except RedcapError as e:
    print(f"❌ Error uploading to REDCap: {e}")
```

### 4. Run the Script

Navigate back to your terminal and run:

=== "Using uv"

    ```bash
    uv run python upload_instruments.py
    ```

=== "Using pip/venv"

    ```bash
    python upload_instruments.py
    ```

Expected output:

```
📋 Preparing to upload 45 fields...
✅ Successfully uploaded 45 fields to REDCap
```

### 5. Verify in REDCap

1. Log in to REDCap
2. Open your project
3. Go to **Online Designer**
4. You should see your uploaded instruments

## Upload Selected Instruments

To upload only specific instruments, modify the import and `pd.concat(...)` list in your script.

!!! tip "Finding available instruments"
    To see all instruments available in `rcol`, check the [Instruments documentation](../instruments/index.md) or run the following in Python:

    ```python
    import rcol.instruments
    import rcol.rtg
    print(dir(rcol.instruments))
    print(dir(rcol.rtg))
    ```

Example with selected RTG instruments:

```python title="upload_selected.py"
from dotenv import load_dotenv
import os
import pandas as pd
from redcap import Project, RedcapError
from rcol.rtg import (
    study_participant_information,
    becks_depression_inventory_ii,
    sf12,
    moca,
)

load_dotenv()
RC_API_KEY = os.getenv("RC_API_KEY")

# Combine selected instruments
instruments = pd.concat([
    study_participant_information,
    becks_depression_inventory_ii,
    sf12,
    moca,
], ignore_index=True)

print(f"📋 Uploading {len(instruments)} fields...")

# Upload
api_url = "https://redcapdev.uol.de/api/"
rc_project = Project(api_url, RC_API_KEY)

try:
    response = rc_project.import_metadata(instruments, import_format="df")
    print(f"✅ Successfully uploaded {response} fields")
except RedcapError as e:
    print(f"❌ Error: {e}")
```

## Handling Existing Instruments

!!! warning "Important"
    Uploading instruments **overwrites** existing metadata. Back up your project first!

### Backup Before Upload

```python
from redcap import Project

rc_project = Project("https://redcapdev.uol.de/api/", RC_API_KEY)

# Export current metadata as backup
metadata = rc_project.export_metadata(format_type="df")
metadata.to_csv("metadata_backup.csv", index=False)
print(f"✅ Backup saved: {len(metadata)} fields")
```

### Export and Review

```python
# View current project structure
metadata = rc_project.export_metadata(format_type="df")
print("Current instruments:")
print(metadata["form_name"].unique())
```

## What's Next?

After uploading your instruments, enable them as surveys:

**[→ Step 6: Enable Survey Mode](survey-mode.md)**

## Troubleshooting

??? question "Error: 'The field_name column contains duplicate values'"
    - Check that you're not uploading duplicate instruments
    - Each field_name must be unique across all instruments
    - Use the validation function above to identify duplicates

??? question "Error: 'You do not have API Import/Update privileges'"
    - Contact your REDCap administrator
    - Request Import/Update permissions for your API token

??? question "Only some fields were uploaded"
    - Check the response message for the actual count
    - Verify field_type values are valid REDCap types
    - Look for special characters in field_label or choices
