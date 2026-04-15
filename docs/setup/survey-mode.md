# Step 6: Enable Survey Mode

This guide explains how to enable your instruments as participant-facing surveys in REDCap.

## Prerequisites

- [x] REDCap project with uploaded instruments ([Steps 1-5](index.md))

## What is Survey Mode?

Survey mode transforms your instruments into web-based questionnaires that participants can complete via a link or QR code. 

| Mode | Who Uses It | How |
|------|-------------|-----|
| **Data Entry** | Researchers | Through REDCap interface |
| **Survey** | Participants | Via public link/QR code |

## Enable Surveys for Your Project

### 1. Enable Survey Functionality

First, enable the survey feature for your entire project:

1. Log in to REDCap
2. Open your project
3. Go to **Project Setup** (left sidebar)
4. Find **"Enable surveys for this project?"**
5. Click **Enable** if not already enabled

### 2. Enable Individual Instruments as Surveys

For each instrument you want to use as a survey:

1. Go to **Online Designer**
2. Find your instrument (e.g., "fal")
3. Click **"Enable as survey"** (gray bubble icon)
4. Configure survey settings
5. Click **Save Changes**

## Configure Survey Settings

### Basic Settings

When enabling a survey, configure these essential settings:

| Setting | Recommended Value | Description |
|---------|-------------------|-------------|
| **Survey Title** | Your instrument name | Shown at the top of the survey |
| **Survey Instructions** | Clear instructions | Displayed before questions |
| **Survey Status** | Active | Must be active to collect responses |

### Example Configuration

```
Survey Title: Fragebogen zur Allgemeinen Leistungsfähigkeit

Survey Instructions:
Bitte beantworten Sie die folgenden Fragen zu Ihrer allgemeinen
Leistungsfähigkeit. Es gibt keine richtigen oder falschen Antworten.
Die Bearbeitung dauert etwa 5-10 Minuten.

Survey Status: ● Active
```

### Response Settings

These settings are found under **Online Designer** → click your instrument → **Survey Settings** → **Survey Access**:

| Setting | Option | Use Case |
|---------|--------|----------|
| **Save & Return** | ✅ Enable | Allow participants to pause and continue |
| **Edit Completed** | ❌ Disable | Prevent response changes |

E-consent settings can be found separately in the **Online Designer**:

| Setting | Option | Use Case |
|---------|--------|----------|
| **E-consent** | ❌ Disable | Unless collecting signatures |

### Appearance Settings

| Setting | Option | Description |
|---------|--------|-------------|
| **Question Numbering** | Auto | Automatic numbering |
| **Hide Title** | ❌ No | Show survey title |
| **Required Field Indicator** | ✅ Yes | Mark required fields with * |

## Get Survey Links

### Public Survey Link

For anonymous surveys open to everyone:

1. Click **Survey Distribution Tools** in the left menu bar
2. Click **Public Survey Link** at the top
3. Copy the **Public Survey URL**

```
https://redcapdev.uol.de/surveys/?s=XXXXXXXXXX
```

### Individual Survey Links

For tracked participants:

1. Click **Survey Distribution Tools** in the left menu bar
2. Click **Participant List**
3. Click **Add Participants** and add your participants by email address
4. Click **Add Participants** to save
5. Click **Compose Survey Invitations** to send links

### QR Codes

Generate QR codes for easy access:

1. Go to **Survey Distribution Tools**
2. Click **QR Code**
3. Download or print the QR code

## Survey Queue (Multiple Instruments)

If using multiple surveys, set up a Survey Queue:

1. Go to **Online Designer**
2. Drag instruments into the desired order using the left-hand side
3. Click **Survey Queue** → **Edit Survey Queue**
4. Configure **Auto start** and **Display order**

Example queue:

```
1. Study Participant Information (auto-start)
2. FAL Questionnaire
3. Edinburgh Handedness Inventory
4. Beck Depression Inventory
```

## Automated Invitations

Set up automated survey invitations:

### Enable Automated Invitations

1. Go to **Online Designer**
2. Click **Automated Invitations** next to your survey
3. Configure logic and scheduling as necessary

### Configure Logic

```
Send invitation:
  When: [survey_1_complete] = '2'  (After first survey complete)
  Delay: 0 days, 0 hours, 0 minutes
  To: Participant's email
```

## Complete Workflow Example

Here's a complete example for a typical research study:

```python title="setup_complete_project.py"
from dotenv import load_dotenv
import os
import pandas as pd
from redcap import Project, RedcapError
from rcol.instruments import fal, ehi
from rcol.rtg import study_participant_information

# Load API key (see Step 5 for how to store it)
load_dotenv()
RC_API_KEY = os.getenv("RC_API_KEY")

# 1. Upload instruments
instruments = pd.concat([
    study_participant_information,
    fal,
    ehi,
], ignore_index=True)

print("📋 Uploading instruments...")
api_url = "https://redcapdev.uol.de/api/"
rc_project = Project(api_url, RC_API_KEY)

try:
    response = rc_project.import_metadata(instruments, import_format="df")
    print(f"✅ Uploaded {response} fields")
except RedcapError as e:
    print(f"❌ Upload failed: {e}")
    raise
    
print("\n✅ Setup complete!")
print("\nNext steps:")
print("1. Log in to REDCap")
print("2. Go to Online Designer")
print("3. Enable each instrument as a survey")
print("4. Configure survey settings")
print("5. Get your survey link from Survey Distribution Tools")
```

## What's Next?

Your REDCap project is now ready for data collection! 

Additional resources:

- [Troubleshooting Guide](troubleshooting.md) - Common issues and solutions
- [Instrument Reference](../instruments/index.md) - View all available instruments

## Troubleshooting

??? question "Survey link shows 'Survey is not active'"
    - Check that surveys are enabled for the project
    - Verify the individual instrument is enabled as a survey
    - Ensure Survey Status is set to "Active"

??? question "Participants can't submit the survey"
    - Check required fields are correctly configured
    - Verify branching logic doesn't hide submit button
    - Test the survey yourself first

??? question "I can't find the Survey Distribution Tools"
    - Enable surveys for the project first
    - You must enable at least one instrument as a survey
    - Check your user rights include survey distribution

??? question "Automated invitations are not sending"
    - Verify the Automated Invitations feature is enabled
    - Check your logic conditions
    - Ensure participant emails are valid
    - Check REDCap's email sending settings with your administrator
