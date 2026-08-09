# Server Tag Monitor by New North (sGTM)

The server container is where tracking quietly breaks: a tag reports success,
the platform rejects the payload, and nothing in GTM says so. This template
reports which tags ran per event, with what result and how long they took, and
checks the events themselves for the parameters the platforms actually require.

It sends that to an HTTPS endpoint **you** configure. The default is the hosted
[New North Digital Tag Monitor](https://monitor.newnorth.nl); point it at your
own collector and the data is yours.

Fork of Simo Ahava's Server Container Monitor template (Apache 2.0,
[gtm-templates-simo-ahava](https://github.com/gtm-templates-simo-ahava)).

## Features

- **Per-tag execution telemetry** via `addEventCallback`: tag id, name, status
  and execution time, per event, plus the sGTM Client that claimed the request.
- **Required-parameter checks on real traffic**: GA4 ecommerce events missing
  `transaction_id`, `value`, `currency` or `items`, and items missing `item_id`
  or `item_name`. Only the **names** of missing parameters are reported, never
  values.
- **Optional conversion value**, off by default, so a failing tag can be reported
  as an amount instead of a count. Business data, not personal data.
- **Sampling** with the rate recorded on every hit, so volumes can be scaled back
  up without breaking historical comparisons.
- **Fire-and-forget delivery**: `gtmOnSuccess()` is called before the HTTP call,
  so monitoring never delays the container's own response.

## Installation

**From the Community Template Gallery:** Templates → Tag Templates → Search
Gallery → "Server Tag Monitor by New North" → Add to workspace.

**Manual:** download `template.tpl` from this repository, then Templates → Tag
Templates → New → ⋮ → Import.

## Setup

1. Create a tag from the template.
2. Set **Collection endpoint** (defaults to the hosted service) and, if your
   collector needs one, a **Site token**.
3. Choose a **Sample rate**. Server containers see every request, so a low rate
   is usually right on high-volume setups.
4. Trigger it on **All Events**.
5. On the monitor tag itself, add tag metadata `exclude` = `true` so it does not
   report on itself.

## Fields

| Field | Description |
|---|---|
| Collection endpoint | HTTPS endpoint receiving the data. Defaults to the hosted service. |
| Site token | Sent as the `t` field. Required by the hosted service, optional for your own collector. |
| Sample rate (%) | Percentage of events that report telemetry (1-100). |
| Report missing required parameters | Checks incoming ecommerce events against the GA4 spec. Names only. |
| Extra requirements (optional) | Additional parameter names you require, per event name. |
| Include the monetary value of conversion events | Off by default. Adds value and currency so losses can be reported as amounts. |
| Verbose logging (per-event) | Debug output. Leave off in production. |

## What it sends

A JSON POST per event:

```json
{
  "t": "<site token>", "cid": "<container>", "cv": "<version>",
  "en": "<event name>", "ets": 1700000000000, "cn": "<claiming client>",
  "sr": 10, "mp": "transaction_id,items",
  "tags": [{ "id": "12", "nm": "Meta CAPI", "st": "success", "et": 88 }]
}
```

`mp` is present only when something required was missing, so it is always a
problem list and never a dump of the event.

## Permissions

| Permission | Why |
|---|---|
| `read_event_metadata` | The per-tag results `addEventCallback` provides. |
| `read_event_data` | Event name and the ecommerce fields the parameter check inspects. |
| `read_container_data` | Container id and version, so a change can be tied to a publish. |
| `send_http` (`https://*`) | Delivering the payload to the endpoint you configure. |
| `logging` | Failure logging; verbose per-event logging is behind the debug field. |

## Resources

- Web leg: [tag-monitor-web-tag](https://github.com/newnorthdigital/tag-monitor-web-tag)
- Hosted service: [monitor.newnorth.nl](https://monitor.newnorth.nl)
- What the hosted service stores: [monitor.newnorth.nl/privacy](https://monitor.newnorth.nl/privacy)

## Author

Created and maintained by [Freek Kampen](https://freekkampen.com) at [New North Digital](https://newnorth.nl).

## License

Apache 2.0. See [LICENSE](LICENSE).
