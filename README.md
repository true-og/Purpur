# true-og/Purpur

A fork of [Purpur](https://github.com/PurpurMC/Purpur) 1.19.4 maintained by [TrueOG Network](https://trueog.net) with small quality-of-life patches for our server. (Purpur is itself a fork of [Paper](https://github.com/PaperMC/Paper).)

## Changes over mainline Purpur

- Never rewrites existing config files (server.properties, bukkit.yml, commands.yml, spigot.yml, pufferfish.yml, purpur.yml, and the Paper configs). Missing files are still created with defaults. Commands that vanilla would save to server.properties (`/whitelist on|off`, `/setidletimeout`) only last until restart.
- Hides "lost connection" messages in the console.
- Keeps chunks loaded while a zombie villager is being cured.

EXPERIMENTAL (may break things):

- Stores player data in RocksDB (`world/playerdatadb`) instead of `.dat` files. A plugin-facing `PlayerDataApi` (reached via `PlayerDataStorage#api()`) exposes `get`/`saveAll`/`has`/`seen`/`drop`/`copy`/`flush`; corrupt stored data refuses the login instead of handing out a fresh profile, and dropping a world with no stored data is treated as success. The replaced revision of the default profile is kept under a `previous` column family as a manual recovery net. Plugins reach all of this through [Utilities-OG](https://github.com/true-og/Utilities-OG), never the fork API directly.

## Building

Clone this repository (do not download it), then:

```
./gradlew applyPatches             # sets up Purpur-API and Purpur-Server
./gradlew createReobfPaperclipJar  # builds the runnable server jar into build/libs
```

Patches are commits in `Purpur-API` / `Purpur-Server`; after editing their source, `./gradlew rebuildPatches` regenerates the patch files under `patches/`. See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## License

All patches are under the MIT license unless noted in a patch header. Purpur © PurpurMC; see [PaperMC/Paper](https://github.com/PaperMC/Paper) and [PaperMC/paperweight](https://github.com/PaperMC/paperweight) for the licenses of the material this project builds on.
