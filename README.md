# TTW.DigitalTwins

[![Build and Copy to Azure](https://github.com/TaylorThomsonWhitting/TTW.DigitalTwins/actions/workflows/build-and-copy-to-azure.yml/badge.svg)](https://github.com/TaylorThomsonWhitting/TTW.DigitalTwins/actions/workflows/build-and-copy-to-azure.yml)

## Build and Develop

Refer to [Terria Development Environment](https://docs.terria.io/guide/contributing/development-environment/) page for details.

TTW.DigitalTwins depends on a custom version of terriajs. To develop locally clone the TaylorThomsonWhitting/terriajs repo into a `packages` directory. The version of the `packages/@ttw/terriajs` package must match the version specified in the TTW.DigitalTwin `package.json`.

An `.npmrc` file with the correct credentials and a scoped @ttw registry must be created for `yarn install` to find the @ttw/terriajs package. (e.g. `@ttw:registry=https://...` etc.).

Running `yarn install` will automatically move the `terriajs` package from `node_modules/@ttw/terriajs` to `node_modules/terriajs` in order to ensure that relative paths are correct.

If doing local development, yarn will attempt to create a symlink to the `terriajs` project in the `/packages` directory, **HOWEVER**, it will probably do it incorrectly, so it is best to run `npm run devlink` to ensure the symlink is correctly established.

Running `npm run gulp` builds both terriajs and TTW.DigitalTwins.

Running `npm run start` serves the site at localhost:3001.

Running `npm run hot` serves the site at localhost:3003 and hot reloads the app when changes are made to either terriajs or TTW.DigitalTwins. You should run `npm run start` in another process so that the hot reload server can locate the initialisiation files from localhost:3001.

## Pulling From Upstream (Updating TerriaJS)

1. Fetch from terriajs upstream
2. Preferably, merge a tagged commit into the `dev` branch.
3. Ensure that the terriajs `package.json` file keeps `"name": "@ttw/terriajs"`.
4. Match the `version` property in the terriajs `package.json` with the tagged version (or closest past version) and format it as `{version}-ttw-1`. e.g. `8.7.0-ttw-1`.
5. Update the terriajs version in TTW.DigitalTwins `package.json` to match the version in step 4.
6. Run `yarn install`.
7. Some dependencies may be out of sync so you may need to run `(npx) gulp sync-terriajs-dependencies`. After this, re-run `yarn install`.
8. Run `npm run gulp`.

## Deploy

### Actions

Any push to this repo will trigger an Azure release pipeline that rebuilds TTW.DigitalTwins, @ttw/terriajs (if required), and redeploys the terriajs-server project.
**Note:** The yarn.lock file should be present and up to date when deploying otherwise the yarn install step may fail.

### Manual

Use `azcopy` to copy the `wwwroot` folder into the `ttwdigitaltwin/$web` Azure Storage account.

```bash
azcopy copy ./wwwroot/* '<SAS URL with write permissions>' --recursive=true --check-length=false
```

# Terria Map

[![Build Status](https://github.com/TerriaJS/TerriaMap/actions/workflows/ci.yml/badge.svg?branch=main&event=push)](https://github.com/TerriaJS/TerriaMap/actions/workflows/ci.yml) [![Docs](https://img.shields.io/badge/docs-online-blue.svg)](https://docs.terria.io/)

![Terria logo](terria-logo.png "Terria logo")

This is a complete website built using the TerriaJS library. See the [TerriaJS README](https://github.com/TerriaJS/TerriaJS) for information about TerriaJS, and getting started using this repository.

For instructions on how to deploy your map, see [the documentation here](doc/deploying/deploying-to-aws.md).

To get in touch:

- Join the [TerriaJS Github Discussion](https://github.com/TerriaJS/terriajs/discussions)
- Raise issues in the [TerriaJS Github issue tracker](https://github.com/TerriaJS/terriajs/issues/new)

---

### We have released TerriaJS v8.3.0 (2023-05-22)

Terriajs version `8.3.0` includes a few breaking changes:

    - Upgrade to Typescript version 4.9.x
    - Upgrade to Mobx version 6.9.x

This might affect your map only if it has local model layer modifications like your own custom data provider (aka catalog items). Otherwise you can proceed like any other normal upgrade. For instructions on upgrading your maps with local modiciations please refer to the [upgrade guide](https://github.com/TerriaJS/terriajs/discussions/6787).

### PM2 no longer supported (2023-03-21)

We've removed pm2 from our dependencies and no longer ship configuration for running terriajs-server with pm2.

`npm start` now runs in forground because it no longer uses pm2. A new task `gulp dev` has been introduced to make development easier. It runs terriajs-server and starts `gulp watch` - which watches for changes and incrementally builds. See https://github.com/TerriaJS/terriajs/discussions/6731 for more information on why and what to do.

### We just reformatted our codebase with [Prettier](https://prettier.io/) (2022-08-29)

This may cause large merge conflicts when you merge `main` into your fork. See https://github.com/TerriaJS/terriajs/discussions/6517 for instructions on how to merge this formatting change.

### We have released TerriaJS v8 (2021-08-13)

What this means:

- [Our new main branch of TerriaMap](https://github.com/TerriaJS/TerriaMap/tree/main) now uses v8+ of TerriaJS
- [The terriajs7 branch of TerriaMap](https://github.com/TerriaJS/TerriaMap/tree/terriajs7) will use v7 TerriaJS, but will not receive further updates
- We have a [migration guide](https://docs.terria.io/guide/contributing/migration-guide/) available for users of TerriaJS v7 to help them upgrade their applications to TerriaJS v8
- Please chat to us and the community in our [GitHub discussions forum](https://github.com/TerriaJS/terriajs/discussions)
