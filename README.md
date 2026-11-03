# PlushCraft Guide

A Fabric decoration mod for **Minecraft Java Edition 26.3**. Ten little mob plushies rest on their bellies with spread-out limbs, oversized heads, stitched fabric details, and original pixel textures.

![Model and texture art preview](docs/plushies.png)

Preview order, left to right: Creeper, Bee, Axolotl, Fox, Frog; Enderman, Slime, Wolf, Cat, Warden. This preview is rendered from the project models and textures by a small art tool; it is not an in-game screenshot.

**Verification:** the Gradle build and Minecraft server integration tests passed. Client visual verification remains pending after a native client exit during resource loading. See [the verification record](docs/TESTING.md) for the exact coverage and remaining checks.

## Requirements

- Minecraft Java Edition **26.3** (not Bedrock).
- **Java 25** development kit to build; Java 25 runtime to play.
- Fabric Loader **0.19.5 or newer**.
- Fabric API **0.165.0+26.3**.
- Gradle **9.6.0**, provided by the included wrapper.
- Fabric Loom **1.17.1**.

Minecraft 26.3 ships unobfuscated. This project uses its original Mojang class and method names directly, with no Yarn or separate mappings dependency. Version choices are pinned in `gradle.properties`.

Reference: [Fabric 26.3 development notes](https://fabricmc.net/2026/09/15/263.html), [official example project](https://github.com/FabricMC/fabric-example-mod/tree/26.3), [Fabric API release](https://github.com/FabricMC/fabric-api/releases/tag/0.165.0%2B26.3).

## Build

Open this folder as a Gradle project in IntelliJ IDEA or another Java IDE. Select JDK 25 for both the project and Gradle JVM. The first build needs an internet connection to download the game, Fabric, and Gradle.

Windows PowerShell:

```powershell
$env:JAVA_HOME = 'C:\path\to\jdk-25'
.\gradlew.bat build
```

macOS / Linux:

```sh
chmod +x gradlew
./gradlew build
```

Install **`build/libs/plushcraft-1.0.0.jar`**. The `-sources.jar` is for developers and is not the playable mod.

Launch a development client with `gradlew.bat runClient` or `./gradlew runClient`. Use `runServer` for a development dedicated server. Client and server use the same mod jar.

## Install and play

1. Install Fabric Loader for Minecraft 26.3 using the [official installer](https://fabricmc.net/use/installer/).
2. Place the PlushCraft jar and the **26.3** build of [Fabric API](https://modrinth.com/mod/fabric-api) into the Minecraft instance's `mods` folder. Install both on a server and its clients.
3. Launch the Fabric profile. Open the **PlushCraft** creative tab, or craft plushies in survival.
4. Right-click the top of a solid supporting block. The plushie's face points toward the player on one of four horizontal directions. To turn an existing plushie, break and place it again.
5. Break a plushie with any tool or an empty hand to recover it. Removing its support also drops it. Creative-mode breaking follows vanilla rules and does not drop an item.

Plushies have no AI, animations, combat, lighting, particles, or special effects. Their collision and selection box is a simple low box, at most 9/16 of a block tall. They require a sturdy upward support face and cannot hang from walls or float.

## Crafting

Each recipe yields **one plushie** from **four matching wool blocks** and **one mob material**, using a crafting table:

```text
· W ·
W M W
· W ·
```

`W` = wool, `M` = material, `·` = empty slot. Acquiring the matching wool unlocks the recipe in the recipe book.

| Plushie | Wool | Material |
| --- | --- | --- |
| Creeper | Green | Gunpowder |
| Bee | Yellow | Honeycomb |
| Axolotl | Pink | Tropical Fish (item, not a bucket) |
| Fox | Orange | Sweet Berries |
| Frog | Green | Slimeball |
| Enderman | Black | Ender Pearl |
| Slime | Lime | Slimeball |
| Wolf | Gray | Bone |
| Cat | White | Raw Cod |
| Warden | Cyan | Sculk |

## Project layout

- `src/main/java/dev/plushcraft/block/PlushBlock.java`: shared facing, shape, placement and support behavior.
- `src/main/java/dev/plushcraft/registry/PlushBlocks.java`: blocks, block items and creative tab.
- `src/main/resources/assets/plushcraft`: models, blockstates, client item definitions, textures and English names.
- `src/main/resources/data/plushcraft`: recipes, recipe unlocks, loot and plushie tags.
- `tools/generate-assets.mjs`: deterministic original artwork and resource generator.
- `tools/validate-assets.mjs`: static resource validation.
- `docs/TESTING.md`: actual verification results and manual test checklist.

Run the server integration tests with `./gradlew build -PgameTests` (Windows: `gradlew.bat build -PgameTests`). The optional client gallery smoke test is `./gradlew runClientGameTest -PclientTests`; it opens Minecraft, creates a temporary test world, places all ten plushies and saves a screenshot under `build/run/clientGameTest/screenshots`.

All resources are committed to the project; Node is optional and is **not** needed to build or play. If changing the models or textures through the generator, use Node 20+:

```sh
node tools/generate-assets.mjs
node tools/validate-assets.mjs
```

The mod uses vanilla cuboid rendering with opaque 64×32 pixel fabric atlases. Wings and gills are padded fabric shapes; the Slime is opaque fabric rather than translucent slime. All plushies fit within one block. No mixins, block entities, custom renderers or additional libraries are used.

## License

Original source code, models and textures are MIT licensed. Minecraft is a trademark of Mojang and Microsoft. PlushCraft is an unofficial fan project and is not affiliated with or endorsed by them.

