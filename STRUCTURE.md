# Public Repository Structure

This is the correct structure for your public Hass.io add-on repository:

```
automation-creator-public/
├── repository.json              # Repository metadata
├── README.md                    # User documentation
└── automation_creator/          # Add-on folder (must match slug in config.yaml)
    └── config.yaml              # Add-on configuration (required!)
```

## Important Notes

1. **Folder name must match slug**: The folder `automation_creator/` must match the `slug` in `config.yaml`
2. **config.yaml is required**: Home Assistant reads this to discover and install the add-on
3. **Image path**: The `image` field in `config.yaml` points to your Docker images on GitHub Container Registry
4. **No source code**: Only metadata files - your code is protected in Docker images

## Files Explained

- **repository.json**: Tells Home Assistant where to find your add-ons
- **automation_creator/config.yaml**: Defines the add-on (name, version, image location, etc.)
- **README.md**: User-facing documentation

