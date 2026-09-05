# Daikin-Airbase-Control
Free Cloud Based App to control your Daikin Airbase

[README.md](https://github.com/user-attachments/files/31858867/README.md)
# Daikin Airbase Cloudflare Controller

A lightweight browser-based controller for compatible **Daikin Airbase** heat-pump systems, hosted on **Cloudflare Workers**.

It provides:

- live indoor and outdoor temperature
- current power state
- current operating mode
- current set temperature
- current fan setting
- manual ON/OFF control
- Heat / Cool / Dry mode selection
- temperature setpoint control
- Auto / Low / Medium / High fan control
- configurable weekly ON/OFF schedules
- a simple responsive web dashboard
- API-key protection for controller endpoints

This version is designed as a standalone generic controller and does not depend on Home Assistant, a Raspberry Pi, or another always-on local server.

## Files

### `daikin-airbase-cloudflare-generic-worker.txt`

The complete Cloudflare Worker source code.

Open the file, copy all of its contents, and paste them into the Cloudflare Worker editor.

### `daikin-airbase-cloudflare-setup-guide.txt`

Step-by-step installation instructions covering:

- finding your Daikin Airbase device details
- creating the Cloudflare Worker
- creating the KV namespace
- adding secrets and variables
- adding the schedule cron trigger
- testing the installation with `curl`
- using the web dashboard
- troubleshooting common setup problems

## Requirements

You will need:

- a compatible Daikin Airbase Wi-Fi controller
- a working Daikin Airbase account
- a Cloudflare account
- your Airbase account/device information

The Worker expects these Cloudflare secrets:

```text
AIRBASE_ID
AIRBASE_PASSWORD
AIRBASE_PORT
AIRBASE_DEVICE
DAIKIN_API_KEY
```

It also requires a KV binding named:

```text
DAIKIN_CONFIG
```

For scheduling, add a cron trigger:

```text
* * * * *
```

A `TIMEZONE` Worker variable is also recommended, for example:

```text
Pacific/Auckland
Australia/Sydney
Europe/London
America/Los_Angeles
```

## Quick start

1. Download both files from this repository.
2. Follow `daikin-airbase-cloudflare-setup-guide.txt`.
3. Create a Cloudflare Worker.
4. Paste the contents of `daikin-airbase-cloudflare-generic-worker.txt` into the Worker editor.
5. Add the required secrets and KV binding.
6. Add the once-per-minute cron trigger.
7. Deploy the Worker.
8. Open the Worker URL in a browser.
9. Expand **API access**, enter your controller API key, and save it.

## Testing

The public health endpoint is:

```bash
curl "https://YOUR-WORKER-URL/health"
```

The authenticated status endpoint is:

```bash
curl "https://YOUR-WORKER-URL/status" \
  -H "Authorization: Bearer YOUR_DAIKIN_API_KEY"
```

If the Daikin details and Cloudflare bindings are correct, the status response should include the current control state and temperature sensors.

## Airbase compatibility

This code was developed against a Daikin Airbase system using the `dascontroller.com/skyfi/aircon` cloud API.

Daikin hardware, firmware, regional services, and account configurations can differ. If you test it successfully with another Airbase model or identify a compatibility issue, please consider posting the details in the discussion where you found this project.

## Security

Do not hard-code your Airbase password or controller API key into the Worker source.

Store these as Cloudflare encrypted secrets:

```text
AIRBASE_PASSWORD
DAIKIN_API_KEY
```

The web dashboard itself is reachable at the Worker URL, but controller operations require the API key. When saved in the dashboard, the key is stored locally in that browser.

## Disclaimer

This is an independent, community-created integration. It is not an official Daikin or Cloudflare product and is not endorsed by either company.

Use it at your own risk. Verify operation carefully on your own equipment before relying on schedules or remote control.
