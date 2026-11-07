# sharedb-client-browser

A distribution of the [ShareDB](https://github.com/share/sharedb) client suitable for use in Vite, Rollup and other build tools that do not have built-in support for CommonJS and Node polyfills. Uses the venerable [Browserify](https://browserify.org/).

There are 3 UMD builds provided:

- `sharedb-client-umd.cjs` Just the ShareDB client
- `ot-json1-presence-umd.cjs` Includes `ot-json1` and `text-unicode` (both exposed to avoid duplication of `text-unicode` in a bundle where application logic needs direct access to it)
- `ot-json1-umd.cjs` Includes `ot-json1` and `text-unicode`

The consuming project needs to put the following in the Vite config (modify depending on which builds you need):

```
  // Required to have Vite properly handle these CommonJS imports
  optimizeDeps: {
    include: [
      'sharedb-client-browser/dist/sharedb-client-umd.cjs',
      'sharedb-client-browser/dist/ot-json1-presence-umd.cjs'
    ]
  },
```

See also

 * [GitHub Issue for ShareDB: Browser build for client](https://github.com/share/sharedb/issues/499)
 * [GitHub Issue for VZCode: Presence cursors](https://github.com/vizhub-studio/vzcode/issues/16)

## Build and integration guide

This repository produces **Browserify UMD bundles** for ShareDB and JSON1-related browser consumption. It is not the upstream ShareDB implementation or a hosted synchronization server.

```bash
npm ci
npm run build
```

The build generates `dist/sharedb-client-umd.cjs`, `dist/ot-json1-umd.cjs`, and `dist/ot-json1-presence-umd.cjs` from [`src/`](src/). Root `package.json` has **no test script**. In consuming Vite projects, add only the prebundling entries actually used and validate OT/ShareDB version compatibility with the consuming application's tests.

The `Upgrading ShareDB` section below documents a **maintainer release procedure**, including publishing and Git mutations. It is not a standard setup command and must not be executed without explicit release authorization.

## Upgrading ShareDB

To cut a new release when ShareDB increments versions:

```
npm run upgrade
npm i
git commit -m "Upgrade ShareDB" -a
# Adjust the following command as needed to match upstream ShareDB version
npm version patch
npm publish
git push
git push --tags
```

Then put a new release in GitHub
