# Quick Start Guide

## Installation (5 minutes)

### 1. Add Repository
**Settings** → **Add-ons** → **Add-on Store** → **⋮** → **Repositories** → **Add**
```
https://github.com/indteam/automation-creator-public
```

### 2. Install Add-on
Search for **"Automation Creator"** → **Install**

### 3. Configure
**Configuration** tab → Enter your OpenAI API key:
```yaml
openai_api_key: "sk-your-key-here"
```
Click **Save** → **Start**

### 4. Add REST Command
Edit `configuration.yaml` (use File Editor add-on), add at the end:
```yaml
rest_command:
  call_automation_creator:
    url: http://127.0.0.1:8099/api/create-automation
    method: POST
    headers:
      Content-Type: application/json
    payload: '{"user_text": "{{ user_text }}" }'
```
**Restart Home Assistant**

### 5. Create Automation Trigger
**Settings** → **Automations** → **Create Automation** → **Edit in YAML** → Paste:

```yaml
alias: Assist Automation Creator
id: assist_automation_creator
mode: single
trigger:
  - command:
      - create automation {anything}
      - make automation {anything}
      - automation {anything}
      - when {trigger} turn on {action}
      - when {trigger} turn off {action}
    platform: conversation
conditions: []
actions:
  - service: persistent_notification.create
    data:
      title: Creating Automation
      message: Processing your request...
  - service: rest_command.call_automation_creator
    data:
      user_text: "{{ trigger.sentence }}"
```

### 6. (Optional) Use OpenAI for Assist replies
If you want Assist replies powered by OpenAI:
1) Settings → **Devices & Services** → **Add Integration** → **OpenAI Conversation** → paste your OpenAI API key
2) Settings → **Voice Assistants** → **Assist/Pipelines** → edit default pipeline → set **Conversation agent** to **OpenAI Conversation** → Save

### 7. Test
Say to Assist: **"Create automation turn on kitchen light when motion detected"**

✅ Done! Check **Automations** to see your new automation.

