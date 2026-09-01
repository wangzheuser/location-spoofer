# Third-party proxy modules

Third-party proxy mode (Surge / Quantumult X / Loon / Shadowrocket / Stash /
Egern) now relies entirely on the upstream
[Yu9191/wloc](https://github.com/Yu9191/wloc) modules. The App no longer
maintains or ships project-owned module/script copies, so it never drifts
from the upstream protocol.

## Subscription addresses

The App builds each client's module subscription URL directly from the
upstream repository:

- default mirror (gh-proxy):
  `https://gh-proxy.org/https://raw.githubusercontent.com/Yu9191/wloc/refs/heads/main/modules/<file>`
- direct:
  `https://raw.githubusercontent.com/Yu9191/wloc/refs/heads/main/modules/<file>`

| Module file | Client |
|---|---|
| `wloc.module` | Shadowrocket |
| `wloc.sgmodule` | Surge and Egern |
| `wloc.conf` | Quantumult X |
| `wloc.lpx` | Loon |
| `wloc.stoverride` | Stash |

No `?v=` cache-bust is appended: the URL points at upstream's latest content,
and re-importing the subscription in the proxy client re-fetches it.

## Script protocol

The upstream `wloc.js` patches Apple WLOC responses and reads coordinates from
the `wloc_settings` persistent key or the module `argument` config. The
upstream `wloc-settings.js` implements `wloc-settings/save` (query/clear/save)
using `lon`/`lat`/`acc`/`randomRadius` parameters.

The App's third-party save sends `lon`/`lat`/`acc`, matching the upstream
script. Motion-state simulation (fields 11/12) is **not** implemented by the
upstream scripts and is unavailable in third-party mode; it remains available
in APP mode (built-in proxy).
