# Tag Monitor by New North (server)

Private sGTM tag template for the NND Tag Monitor product ([monitor.newnorth.nl](https://monitor.newnorth.nl)). Reports per-tag firing telemetry from a client's server container to the Tag Monitor ingest endpoint.

**Private template — do not submit to the Community Template Gallery.** It only works with a site token issued by New North Digital.

Fork of Simo Ahava's Server Container Monitor template (Apache 2.0, [gtm-templates-simo-ahava](https://github.com/gtm-templates-simo-ahava)).

## Install (client container)

1. Download `template.tpl` from this repository.
2. In the client's GTM server container: **Templates → Tag Templates → New → ⋮ → Import**.
3. Create a tag from the template, set the client's **Site token**, and fire it on the events to monitor.

## Fields

| Field | Description |
|---|---|
| Site token | Per-site token issued by NND, identifies the client in the monitor |
| Sample rate | Percentage of requests that report telemetry |
| Debug logging | Console logging for troubleshooting |

Sends beacons to `https://monitor.newnorth.nl/api/e`.

## Related

- Web leg: [tag-monitor-web-tag](https://github.com/newnorthdigital/tag-monitor-web-tag)
- Product: `~/tag-monitor` (monitor.newnorth.nl — pixel ingest → BigQuery + cron alerting)

## Author

Created and maintained by [Freek Kampen](https://freekkampen.com) at [New North Digital](https://newnorth.nl).

## License

Apache 2.0. See [LICENSE](LICENSE).
