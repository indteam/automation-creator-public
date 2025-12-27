# Automation Creator Add-on

AI-powered automation creation for Home Assistant using natural language. Supports both English and Russian.

**Version 1.0.8** - Now with room/area context inference and Russian language support!

## Quick Setup Guide

### Step 1: Add Repository

1. Go to **Settings** → **Add-ons** → **Add-on Store**
2. Click the three dots (⋮) in the top right → **Repositories**
3. Click **Add**
4. Enter: `https://github.com/indteam/automation-creator-public`
5. Click **Add**

### Step 2: Install Add-on

1. In the Add-on Store, search for **"Automation Creator"**
2. Click on **Automation Creator**
3. Click **Install**
4. Wait for installation to complete

### Step 3: Configure

1. Go to the **Configuration** tab
2. Enter your OpenAI API key:
   ```yaml
   openai_api_key: "sk-your-api-key-here"
   ```
3. Click **Save**

### Step 4: Start Add-on

1. Go to the **Info** tab
2. Click **Start**
3. Wait a few seconds, then check the **Logs** tab
4. You should see: `"Using Supervisor token"` and `"Home Assistant client initialized"`

### Step 5: Set Up Automation Trigger

1. Go to **Settings** → **Automations & Scenes** → **Automations**
2. Click **Create Automation**
3. Click the three dots (⋮) → **Edit in YAML**
4. Paste this automation:

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

5. Click **Save**

### Step 6: Add REST Command

1. Install **File Editor** add-on (if you don't have it):
   - Settings → Add-ons → Add-on Store
   - Search "File editor"
   - Install and start it

2. Open File Editor and edit `configuration.yaml`

3. Add this at the end of the file:

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

**Note:** The IP address `127.0.0.1` (localhost) is used because the add-on runs on the same host as Home Assistant. If your Home Assistant is running on a different machine, replace `127.0.0.1` with the actual IP address of the machine where Home Assistant is running.

4. **Save** the file

5. Go to **Developer Tools** → **YAML** → **Check Configuration**

6. If valid, restart Home Assistant:
   - Settings → System → Restart

### Step 7: (Optional) Add Automation Modification Trigger

If you want to modify automations via Assist:

1. Go to **Settings** → **Automations & Scenes** → **Automations**
2. Click **Create Automation**
3. Click the three dots (⋮) → **Edit in YAML**
4. Paste this automation:

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

5. Click **Save**

### Step 8: Test It!

**Create an automation (English):**
1. Open **Assist** (or use voice)
2. Say: **"Create automation turn on kitchen light when motion detected"**
3. Wait a few seconds
4. Check **Settings** → **Automations & Scenes** → **Automations**
5. You should see a new automation!

**Create an automation (Russian):**
1. Open **Assist** (or use voice)
2. Say: **"Создай автоматизацию включить свет когда движение"** or **"Сделай так чтобы когда я в душе, включался основной свет"**
3. Wait a few seconds
4. Check **Settings** → **Automations & Scenes** → **Automations**
5. You should see a new automation!

**Modify an automation (English):**
1. Open **Assist** (or use voice)
2. Say: **"Modify the kitchen light automation to work only in evenings"**
3. Wait a few seconds
4. Check the automation - it should be updated!

**Modify an automation (Russian):**
1. Open **Assist** (or use voice)
2. Say: **"Измени автоматизацию кухня добавить условие вечер"**
3. Wait a few seconds
4. Check the automation - it should be updated!

---

## Optional: Use OpenAI Conversation Agent for Assist

The add-on works with the default local agent. If you want Assist replies powered by OpenAI:

1. **Install OpenAI Conversation integration**
   - Settings → **Devices & Services** → **Add Integration**
   - Search **OpenAI Conversation**
   - Paste the same OpenAI API key you used for the add-on
   - Finish setup (choose model if prompted)

2. **Make OpenAI the conversation agent in your pipeline**
   - Settings → **Voice Assistants** → **Assist** (or **Pipelines**)
   - Edit your default pipeline → **Conversation agent** → select **OpenAI Conversation**
   - Save

3. **Test**
   - Open Assist and ask: “What can you do?” or create an automation as before

Note: This only changes who replies in Assist. Automation creation still goes through the add-on.

## Troubleshooting

### Add-on won't start
- Check **Logs** tab for errors
- Verify OpenAI API key is correct

### "Cannot create automation" or "Cannot modify automation" error
- Check add-on **Logs** tab
- Verify add-on is **Running**
- Check Home Assistant logs for errors
- Verify the automation exists (for modification requests)

### Automation trigger not working
- Verify the automation is **Enabled**
- Check that `rest_command` was added to `configuration.yaml`
- Restart Home Assistant after adding `rest_command`

## Support

For issues or questions, please check the add-on logs or contact support.
