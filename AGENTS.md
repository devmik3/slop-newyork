# Slop New York

## Development

- Static HTML, CSS, and JavaScript; no package install or bundler is needed.
- `index.html` imports Three.js from jsDelivr and then loads the ordered `js/`
  scripts. Those scripts share `window.NYC`; preserve their dependency order.
- `.agents/setup` checks the Node.js, Python 3, and curl tools supplied by Amp
  orbs. No secrets, databases, or resume hook are required.
- Start the supervised preview with `amp orb services ensure`. Share the portal
  URL it returns, not the internal listening address.
- Browser rendering needs external CDN access. Node tests use local fixtures
  and do not establish that WebGPU rendering works in a browser.

## Verification

From the repository root:

```sh
node --test tests/*.test.cjs tests/*.test.mjs
```

Tests use Node's built-in test runner. The geodata test builds in a temporary
directory; the Overpass tests stub network requests. There is no separate
configured lint or build command.

## Geographic data

`js/nyc-geodata.js` is generated and already tracked. Normal setup must not
regenerate it or refresh the source data. If a task changes geographic data,
edit the appropriate `tools/geo-*.mjs` source or `tools/data/` input and rebuild:

```sh
node --max-old-space-size=8192 tools/build-geodata.mjs
```

`tools/overpass.sh` fetches external data; it is not part of setup or tests.
