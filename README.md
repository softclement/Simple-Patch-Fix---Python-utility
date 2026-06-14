# Simple Patch Utility

A lightweight Python utility for applying quick text-based patches to configuration files, application files, and deployment artifacts.

This script was created for personal use during application support, troubleshooting, PoC activities, and deployment tasks where a small file modification is required without using additional libraries or complex frameworks.

## Features

* Simple text replacement
* Supports multi-line patches
* Creates automatic timestamped backups
* No external dependencies
* Uses only Python standard libraries
* Suitable for Linux, Windows, WSL, and Docker environments

## Example Use Case

Patch a Docker Compose health check configuration:

Before:

```yaml
interval: 5s
timeout: 5s
retries: 10
```

After:

```yaml
interval: 10s
timeout: 10s
retries: 15
start_period: 30s
```

## Script

```python
import shutil
from datetime import datetime

FILE_NAME = "docker-compose.yml"

OLD = """
      interval: 5s
      timeout: 5s
      retries: 10"""

NEW = """
      interval: 10s
      timeout: 10s
      retries: 15
      start_period: 30s"""

with open(FILE_NAME, "r") as f:
    content = f.read()

if OLD in content:

    timestamp = datetime.now().strftime("%d%m%y%H%M%S")
    backup_file = f"{FILE_NAME}_{timestamp}.bak"

    shutil.copy2(FILE_NAME, backup_file)

    content = content.replace(OLD, NEW)

    with open(FILE_NAME, "w") as f:
        f.write(content)

    print(f"Patched OK")
    print(f"Backup: {backup_file}")

else:
    print("Pattern not found. No changes made.")
```

## Backup Naming Convention

The script automatically creates a backup before applying any changes.

Example:

```text
docker-compose.yml_140626223015.bak
application.conf_140626223122.bak
settings.ini_140626223245.bak
```

Format:

```text
<filename>_DDMMYYHHMMSS.bak
```

## Execution

```bash
python patch.py
```

## Notes

* The patch is applied only if the exact text block is found.
* If the pattern is not found, no changes are made.
* A backup is always created before modifying the file.
* Designed for quick operational fixes and personal reference.

## Disclaimer

Always validate changes in a non-production environment before applying them to production systems. Review generated backups and maintain appropriate change management procedures.
