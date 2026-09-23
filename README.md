# About

**NightCore** is a lightweight library with a wide range of utilities and tools for faster and more efficient plugin development.

This library is required for all [NightExpress](https://www.spigotmc.org/resources/authors/81588/)'s plugins.

## ⚠️ Fork Notice — external skin server compatibility

`nightcore-spigot-bs-fix` is a maintenance fork of **NightCore 2.16.4** for servers running **Paper 26.x together with an external skin / authentication system** (e.g. **Blessing Skin**, Yggdrasil Connect).

**Fixed:** an external skin URL (e.g. `https://skin.cauc.fun/...`) stored in `last_skin_url` was passed to `PlayerTextures#setSkin()`, which Paper rejects with `IllegalArgumentException: Expected host 'textures.minecraft.net' but got '...'`, breaking `AsyncPlayerPreLoginEvent` (player login). NightCore now calls `PlayerTextures#setSkin()` **only** for official Mojang texture URLs and silently skips every other host. The URL itself is still stored.

### 注意事项 / Caveats

- **外置皮肤不会通过 Bukkit Profile 生效。** 本修复只保证不报错、不阻断登录；`skin.cauc.fun` 等外置皮肤的显示仍由外置皮肤/登录系统自行处理。
- **不要清空 `last_skin_url`。** 数据库结构未改，NightCore 继续保存原始外置 URL，只是在 Paper 无法处理时跳过恢复。
- **不要把判断改成大小写不敏感**（例如 `equalsIgnoreCase`）。Paper 内部是 `url.getHost().equals("textures.minecraft.net")`，改成忽略大小写会让带大写域名的 URL 重新触发异常。
- **非法/相对 URL 的行为与修复前完全一致**：`not-a-valid-url` 这类非绝对 URI 仍会抛 `IllegalArgumentException`，本次只做调用前判断、刻意没有扩大 `catch`。
- **其他 `setSkin` 调用点未修改**：`PlayerProfiles#createStaticTexturedProfile(...)` 与 `ItemUtil`（头颅皮肤）仍只接受 `textures.minecraft.net/texture/...`。
- **编译环境：JDK 25。** `ExcellentEconomy 2.8.0` 的 class file 版本为 69.0，用 JDK 21 编译会报 `class file has wrong version 69.0, should be 65.0`。
- **构建产物名：** 根 `pom.xml` 增加了 `<name>nightcore-spigot-bs-fix</name>`（仅影响显示名与 shade 输出文件名，不影响 `groupId`/`artifactId`），构建结果为 `target/nightcore-spigot-bs-fix-2.16.4.jar`。
- **合并上游 NightCore 更新时**注意 `main/src/main/java/su/nightexpress/nightcore/userdata/UserData.java` 的冲突点。

完整根因、diff、验证数据与构建步骤见 [FIX-REPORT.md](FIX-REPORT.md)。

## Features

✅ What exactly is included in these 1.4 MB:
- **Pure original code** written by a human, not by AI.
- **Server Bridge**, providing simultaneous support for **Spigot**, **Paper** and **Folia**.
- **Economy Bridge**, providing simultaneous support for economies/currencies from multiple plugins.
- **Item Bridge**, providing simultaneous support for custom items from multiple plugins.
- **Permissions Bridge**, providing support for various permission plugins.
- **Custom Text Component Parser** with **Spigot** and **Paper** support.
- **Custom Placeholder Parser** featuring "lazy" replacement for maximum efficiency.
- **Command Tools** for creating commands, custom argument types, and tab-completion.
- **YAML Config Tools** for creating config "schemas" with automated reading/writing of paths, values, and comments.
- **Localization Tools** for creating localization "schemas" with automated reading/writing of paths, values, and parameters.
- **Database Tools** for SQLite and MySQL, including SQL query wrappers and table data synchronization.
- **Dialog Screen Tools** for creating interactive dialog screens.
- **Inventory GUI Tools** for creating custom inventory menus.
- **Player Utilities** for handling `Player` objects.
- **Entity Utilities** for handling `Entity` objects.
- **Location Utilities** for handling `Location` objects.
- **Number Utilities** (parsing, rounding, etc.).
- **Randomization Utilities** (utilizing the new `RandomGenerator`).
- **Time Utilities** (`LocalTime`, various formatting options).
- **ItemStack Utilities** for handling `ItemStack` objects.
- **Enum Utilities** for working with `enum` types.
- **String Utilities** for text manipulation.
- **PersistentDataContainer Utilities** for easier data storage.
- **Reflection Utilities** for advanced backend tasks.
- **Bukkit Wrappers** for simpler and more convenient interaction with Bukkit objects.
- **GameProfile Wrapper & Cache** for fast access to player skins and custom heads.
- **Player-Placed Block Tracker** (uses native world chunk storage, no unnecessary databases).

❌ What is NOT included:
- **No "bloatware" libraries** for every minor task.
- **No data collectors** or analytics.
- **No update checkers** or network access.
- **No licensing systems** or activation keys.
- **No advertisements**.

## Links
- [Modrinth](https://modrinth.com/plugin/nightcore)
- [Hangar](https://hangar.papermc.io/NightExpress/nightcore)
- [Documentation](https://nightexpressdev.com/nightcore/)
- [Developer API](https://nightexpressdev.com/nightcore/developer-api/)

## Plugins
Plugins powered by **NightCore**.

- [AdvancedDungeonArena](https://nightexpressdev.com/dungeon-arena/)
- [CoinsEngine](https://nightexpressdev.com/coinsengine/)
- [CombatPets](https://www.spigotmc.org/resources/100360/)
- [DivineSkills](https://www.spigotmc.org/resources/93015/)
- [ExcellentClaims](https://www.spigotmc.org/resources/119848/)
- [ExcellentCrates](https://nightexpressdev.com/excellentcrates/)
- [ExcellentEnchants](https://www.spigotmc.org/resources/61693/)
- [ExcellentJobs](https://www.spigotmc.org/resources/114783/)
- [ExcellentQuests](https://www.spigotmc.org/resources/107283/)
- [ExcellentShop](https://www.spigotmc.org/resources/50696/)
- [LootConomy](https://www.spigotmc.org/resources/83994/)
- [SunLight](https://www.spigotmc.org/resources/67733/)

## Donate
If you like my work or enjoy using my plugins, feel free to [Buy me a coffee](https://ko-fi.com/nightexpress) :) Thank you! 🧡