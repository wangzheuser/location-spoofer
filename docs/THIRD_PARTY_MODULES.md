# Third-party proxy modules

The files under `Resources/ThirdPartyProxyModules/` are project-owned module
definitions used by the App's default domestic-mirror subscription path. Their
executable scripts are built from `ThirdParty/WlocScripts/src/` and checked in
under the versioned `ThirdParty/WlocScripts/dist/v1/` directory.

The direct GitHub Raw variants are stored under
`ThirdParty/WlocScripts/modules/direct/`. The Settings switch selects which
module URL the App copies:

- enabled by default: `gh-proxy.org` in front of GitHub Raw;
- disabled: GitHub Raw directly.

The App appends the module version as a query parameter, such as `?v=1.0.1`.
Increment this value whenever an existing module path changes so proxy clients
do not reuse a previously cached subscription body.

The Apple WLOC response rule and MITM hostname list cover all known network
location endpoints: `gs-loc.apple.com`, `gs-loc-cn.apple.com`,
`gsp-ssl.ls.apple.com`, `bluedot.is.autonavi.com`, and
`bluedot.is.autonavi.com.gds.alibabadns.com`. The settings endpoint
(`/wloc-settings/*`) stays scoped to the `gs-loc` hostnames.

| Module file | Client |
|---|---|
| `wloc.module` | Shadowrocket |
| `wloc.sgmodule` | Surge and Egern |
| `wloc.conf` | Quantumult X |
| `wloc.lpx` | Loon |
| `wloc.stoverride` | Stash |

Egern reuses the Surge module. Stash imports `.stoverride` directly.

Both script entry points are owned by this repository:

- `wloc.js` patches Apple WLOC response bodies;
- `wloc-settings.js` implements `/wloc-settings/save` and
  `/wloc-settings/version`.

The generated scripts target ES2017 and avoid hard dependencies on `BigInt`,
optional chaining, nullish coalescing, `globalThis`, `URLSearchParams`, and
`Object.fromEntries`. This keeps them usable by older proxy-client releases
that still implement the established module syntax, binary response body,
`$done`, and persistent-storage APIs.

Already-installed legacy modules that point to another repository are not
silently migrated because their remote script URL is outside this project's
control. Those users must re-import the project-owned module before using the
versioned protocol and motion setting.

Current bundled module SHA-256 values:

```text
1987adf691738544ee44ee67749872ac3ef36efbe54cd8751cf874eaa029d7f4  wloc.conf
5562020a8de1a25e3162584fa77c50d8c7e1052cdc08c068180fc2e46d69e7da  wloc.lpx
cad43570ffb0a16cf3ba7d06d9c0d99ea1b7994ff665eea192011961f1385e87  wloc.module
96be7cc7e710436ffa320ccc2e2d45d55d9db7fe26dc6a5bab1fa54333102a99  wloc.sgmodule
0cefbabf294acff3382c575578ed7e4c84fa16242cf0808df581aec004ca8058  wloc.stoverride
```

The project acknowledges [Yu9191/wloc](https://github.com/Yu9191/wloc) as a
reference for earlier WLOC implementation ideas. That acknowledgement is not
an executable dependency or subscription source.
