# Automation Creator Add-on

AI-powered automation creation for Home Assistant using natural language.

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
```

**Note:** The IP address `127.0.0.1` (localhost) is used because the add-on runs on the same host as Home Assistant. If your Home Assistant is running on a different machine, replace `127.0.0.1` with the actual IP address of the machine where Home Assistant is running.

4. **Save** the file

5. Go to **Developer Tools** → **YAML** → **Check Configuration**

6. If valid, restart Home Assistant:
   - Settings → System → Restart

### Step 7: Test It!

1. Open **Assist** (or use voice)
2. Say: **"Create automation turn on kitchen light when motion detected"**
3. Wait a few seconds
4. Check **Settings** → **Automations & Scenes** → **Automations**
5. You should see a new automation!

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

### "Cannot create automation" error
- Check add-on **Logs** tab
- Verify add-on is **Running**
- Check Home Assistant logs for errors

### Automation trigger not working
- Verify the automation is **Enabled**
- Check that `rest_command` was added to `configuration.yaml`
- Restart Home Assistant after adding `rest_command`

## Support

For issues or questions, please check the add-on logs or contact support.
