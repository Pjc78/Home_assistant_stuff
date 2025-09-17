# Eufy Motion → Snapshot → Mobile Push (Home Assistant Blueprint)

This blueprint automates snapshots from a **Eufy camera** when motion is detected.  
It works with the [Eufy Security community integration](https://github.com/fuatakgun/eufy_security) and sends images to your phone via the **Home Assistant Companion App**.

---

## Features
- Triggered by any **motion/person detected binary sensor**.
- Starts/stops Eufy livestream (RTSP or normal) if required for snapshots.
- Waits for a short **warm-up delay** before capturing a frame.
- Saves snapshots under `/config/www/<subfolder>`.
- Pushes the snapshot as an image notification to your mobile device.
- Configurable cooldowns, notification text, and optional click actions.

---

## Installation

1. **Copy the blueprint**
   Save [`eufy_motion_snapshot.yaml`](./eufy_motion_snapshot.yaml) into:
   ```
   config/blueprints/automation/eufy_motion_snapshot.yaml
   ```

2. **Reload Blueprints**
   In Home Assistant, go to:
   ```
   Settings → Automations & Scenes → Blueprints → Import Blueprint → Select File
   ```

3. **Create an Automation**
   - Choose **“Eufy Motion → Snapshot → Mobile Push”**.
   - Fill in the inputs (see below).

---

## Blueprint Inputs

| Input                  | Description                                                                                   |
|-------------------------|-----------------------------------------------------------------------------------------------|
| **Motion sensor**      | Eufy `binary_sensor` that turns **on** when motion/person is detected.                        |
| **Camera entity**      | Eufy `camera` entity to snapshot from.                                                        |
| **Notify service**     | Your Companion App notify service (e.g. `notify.mobile_app_johns_iphone`).                    |
| **Device serial**      | The Eufy device SN (see **Settings → Devices & Services → Eufy device**).                     |
| **Use RTSP**           | Use `start_rtsp_livestream` / `stop_rtsp_livestream` services. Disable for normal livestream. |
| **Start/stop stream**  | Whether to start a livestream before and stop after snapshots. Some cams require this.        |
| **Warm-up delay**      | Delay (in seconds) between starting the stream and taking a snapshot. Default: `2`.           |
| **Cooldown**           | Minimum time between triggers. Default: `0` (no cooldown).                                    |
| **Hold for motion**    | Require motion to persist for a duration before triggering. Default: `0`.                     |
| **Snapshot subfolder** | Subfolder under `/config/www/` where images are stored. Default: `snapshots`.                 |
| **Filename prefix**    | Prefix used in snapshot filenames. Default: `eufy`.                                           |
| **Stop stream after**  | Stop the stream after snapshots. Saves battery/bandwidth. Default: `true`.                     |
| **Notification title** | Title shown in push notification. Default: `Eufy Motion Alert`.                               |
| **Notification text**  | Message body for the notification. Default: `Motion detected`.                                |
| **Click action**       | (Optional) Lovelace view or URL to open when tapping the notification.                        |

---

## Folder Setup

Make sure the snapshot folder exists.  
If using the default (`snapshots`):

```bash
mkdir -p /config/www/snapshots
```

> 📂 Snapshots will be served at `/local/snapshots/...` so your phone can fetch them.

If you want Home Assistant to auto-create the folder, add this to `configuration.yaml`:

```yaml
shell_command:
  ensure_snapshots_dir: "mkdir -p /config/www/snapshots"
```

…and uncomment the `shell_command.ensure_snapshots_dir` step in the blueprint.

---

## External Access

For mobile notifications to show the image:

- **Local network**: `/local/...` works when your phone is on the same Wi-Fi.
- **Remote access**: set an **External URL** under  
  `Settings → System → Network`  
  or use **Nabu Casa Remote UI**.

---

## Tips & Troubleshooting

- **Blank images?** Increase the warm-up delay (e.g. 3–4 seconds).
- **Too many alerts?** Add a cooldown (e.g. 30s–1m).
- **Want only person detection?** Use `binary_sensor.<camera>_person_detected` instead of motion.
- **Missing notify service?** Check **Developer Tools → Services** and look for `notify.mobile_app_...`.

---

## Example Notification

Motion detected at your Eufy camera → snapshot sent directly to your phone.

---

## Credits

- [Home Assistant](https://www.home-assistant.io/)  
- [Eufy Security Integration](https://github.com/fuatakgun/eufy_security)  

---
