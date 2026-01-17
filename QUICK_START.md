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
  call_automation_modifier:
    url: http://127.0.0.1:8099/api/modify-automation
    method: POST
    headers:
      Content-Type: application/json
    payload: '{"user_text": "{{ user_text }}" }'
```

**Note:** The IP address `127.0.0.1` (localhost) should match the host where Home Assistant is running. If Home Assistant is on a different machine, replace `127.0.0.1` with that machine's IP address.

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
      - создай автоматизацию {anything}
      - сделай автоматизацию {anything}
      - сделай так чтобы {anything}
      - автоматизация {anything}
      - когда {trigger} включи {action}
      - когда {trigger} выключи {action}
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

### 6. (Optional) Add Automation Modification Trigger
If you want to modify automations via Assist, create another automation:

**Settings** → **Automations** → **Create Automation** → **Edit in YAML** → Paste:

```yaml
alias: Assist Automation Modifier
id: assist_automation_modifier
mode: single
trigger:
  - command:
      - modify automation {anything}
      - edit automation {anything}
      - change automation {anything}
      - update automation {anything}
      - add condition to {anything}
      - modify {anything} automation
      - edit {anything} automation
      - change {anything} automation
      - измени автоматизацию {anything}
      - редактируй автоматизацию {anything}
      - обнови автоматизацию {anything}
      - добавь условие к {anything}
      - измени {anything} автоматизацию
      - редактируй {anything} автоматизацию
    platform: conversation
conditions: []
actions:
  - service: persistent_notification.create
    data:
      title: Modifying Automation
      message: Processing your modification request...
  - service: rest_command.call_automation_modifier
    data:
      user_text: "{{ trigger.sentence }}"
```

### 7. (Optional) Use OpenAI for Assist replies
If you want Assist replies powered by OpenAI:
1) Settings → **Devices & Services** → **Add Integration** → **OpenAI Conversation** → paste your OpenAI API key
2) Settings → **Voice Assistants** → **Assist/Pipelines** → edit default pipeline → set **Conversation agent** to **OpenAI Conversation** → Save

### 8. Test
**Create automation (English):** Say to Assist: **"Create automation turn on kitchen light when motion detected"**

**Create automation (Russian):** Say to Assist: **"Создай автоматизацию включить свет когда движение"** or **"Сделай так чтобы когда я в душе, включался основной свет"**

**Modify automation (English):** Say to Assist: **"Modify the kitchen light automation to work only in evenings"**

**Modify automation (Russian):** Say to Assist: **"Измени автоматизацию кухня добавить условие вечер"**

✅ Done! Check **Automations** to see your new or modified automation.

## Troubleshooting

### Modification not working?
- **Check add-on logs:** Settings → Add-ons → Automation Creator → Logs
- **Verify Home Assistant version:** The add-on is compatible with Home Assistant 2025.9+ and 2025.12+
- **Error "404" in logs?** Ensure you're using the latest add-on version (v1.0.8+)
- The add-on uses documented API endpoints that work across different Home Assistant versions

### Other issues?
- See the full [README.md](README.md) for detailed troubleshooting

