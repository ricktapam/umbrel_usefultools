# Useful Tools — Umbrel Community App Store

A community-maintained collection of useful self-hosted apps for [Umbrel](https://umbrel.com/). Each app is packaged independently, making it easier to add, update, maintain, and remove apps without restructuring the store.

This is an independent community project, not an official Umbrel release or affiliated with any of the packages included.

## Repository structure

```text
.
├── umbrel-app-store.yml
├── useful-tools-grimmory/
│   ├── umbrel-app.yml
│   └── docker-compose.yml
├── docs/
│   └── new-app-template/
└── README.md
```

- `umbrel-app-store.yml` defines the store ID and display name.
- Each app directory contains its manifest and Docker Compose configuration.
- `docs/new-app-template/` contains starting examples for future apps.

## Installation

1. Create a public GitHub repository for the store.
2. Upload the contents of this repository to its root. Do not add an extra enclosing directory.
3. Update the `submission` field in `useful-tools-grimmory/umbrel-app.yml` with the repository URL.
4. In Umbrel, open **App Store → Community App Stores** and add the repository URL.
5. Open **Useful Tools** and install Grimmory.

See the [official Umbrel Community App Store template](https://github.com/getumbrel/umbrel-community-app-store) for the underlying format and requirements.

## Adding another app

1. Choose a unique, permanent app ID using the `useful-tools-` prefix. For example, `useful-tools-kitchenowl`.
2. Create a top-level directory with exactly that ID.
3. Add `umbrel-app.yml` and `docker-compose.yml`, using the examples in `docs/new-app-template/` as a starting point.
4. Complete the manifest with the app's name, description, developer, website, icon, version, and support information.
5. Configure the Docker services using the upstream project's official deployment instructions.
6. Configure Umbrel's `app_proxy` service to target the correct app container and web port. Follow the current official template for service naming and proxy configuration.
7. Give the app its own persistent data directory under `${APP_DATA_DIR}`. Do not reuse another app's data, credentials, database, or service names.
8. Test installation, access, restarts, persistence, and updates before merging the change.

**No central app list needs updating.** Umbrel discovers apps from their individual directories and manifests.

## App ID conventions

The store ID and app IDs are different concepts:

- Store ID: `useful-tools`
- Example app ID: `useful-tools-grimmory`
- Future app IDs: `useful-tools-kitchenowl`, `useful-tools-bookstack`

The directory name must match the app manifest's `id`, and the app ID must use the store ID as its prefix.

Treat published app IDs as permanent. Changing an ID after installation may cause Umbrel to treat the app as a separate installation rather than an update.

## Grimmory

- The Docker image is pinned to a specific release for predictable deployments.
- Application and database data are stored persistently under Umbrel-managed app storage.
- The `/books` and `/bookdrop` paths are intended for library files and incoming books.
- Host-folder access depends on the volume mappings and folder permissions configured for the app. Document any additional Umbrel folder-access settings required by the package.
- Port `6061` is optional and is only needed if an external reverse proxy, such as Nginx Proxy Manager, requires a host-accessible port. Umbrel's built-in app proxy does not require it.
- Back up the database and library files before major upgrades or storage changes.

## Updating apps

For each update:

1. Check the upstream project's release notes and migration instructions.
2. Update the image tag or digest in the app's Compose file.
3. Increment the app's `version` in `umbrel-app.yml`.
4. Update the app's release notes or changelog.
5. Test the updated package before publishing it.

A repository commit alone does not guarantee that an installed app immediately pulls a new image. Use Umbrel's supported update or redeployment workflow.

## Security and maintenance

- Never commit passwords, API tokens, private keys, or personal domain settings.
- Prefer version-pinned images, and use image digests where practical.
- Keep databases on internal Docker networks; do not publish database ports unless necessary.
- Use unique credentials and persistent storage for each app.
- Document breaking changes, migrations, backups, and recovery procedures.
- Verify third-party images and their maintenance status before adding them.
- Test changes on an Umbrel installation before describing an app as verified.

## Contributing

Contributions are welcome. Before submitting a change, check the upstream project's documentation, follow the existing app structure, and test the installation and persistence behavior.

Keep packages minimal, self-contained, and easy for other Umbrel users to maintain.