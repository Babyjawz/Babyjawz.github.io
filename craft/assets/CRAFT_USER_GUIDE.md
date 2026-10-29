# Nymphs -Craft User Guide

Nymphs -Craft lets you choose which Skyrim crafting recipes appear and what ingredients they consume. It reads your active Mod Organizer 2 profile, saves your decisions, and builds a separate plugin called `CraftPatch.esp`.

**The workflow is: select recipes, save decisions, build the patch, enable it in MO2, then check the result in Skyrim.** Saving a decision in Craft does not update the game until you build and enable the resulting patch.

## Requirements

- Windows x64.
- Skyrim Special Edition or Anniversary Edition.
- Mod Organizer 2 with an active Skyrim profile.
- A writable MO2 instance folder containing `ModOrganizer.ini`, `profiles`, and `mods`.
- The correct Skyrim game path configured in MO2, with the game's `Data` folder available.

The self-contained release includes its .NET runtime. Players do not need to install a separate .NET or Visual C++ runtime. Craft itself does not require SKSE, SkyPatcher, SPID, or KID; requirements of your source mods still apply.

These instructions use MO2's instance layout. Craft currently reads the `profiles` and `mods` folders directly under the instance folder. A Vortex-managed setup or an MO2 configuration with those folders redirected elsewhere is outside this workflow.

## Install and launch

1. Extract the complete **Nymphs -Craft** release folder into the `tools` folder inside your MO2 instance.
2. Keep `Nymphs -Craft.exe` and its `app` subfolder together. Copy the whole folder when moving the tool.
3. In MO2's executable settings, add the top-level `Nymphs -Craft.exe`.
4. If the release folder is outside the MO2 instance, set its executable arguments to `--instance "C:\path\to\your\MO2 instance"`, using your actual instance path.
5. Select the Skyrim profile you want to edit in MO2, launch Craft from MO2, and wait for **Ready**. Check the profile name in Craft's status line.

An example layout is:

```text
MO2 instance\
  ModOrganizer.ini
  profiles\
  mods\
  tools\
    Nymphs -Craft\
      Nymphs -Craft.exe
      app\
```

Craft finds the instance by searching its parent folders for the expected MO2 files and folders. The `--instance` argument supplies that folder explicitly.

## Make your first patch

1. Open **Bulk decisions** and select one source mod. Starting with one mod makes the result easy to review.
2. Double-click the source mod to open **Inspect recipes** and look at its entries.
3. Select one recipe. Click **Set visibility…** in **Selected recipe**, choose how many different ingredient types you must carry, and apply the rule.
4. Click **Build patch**. Wait for the message confirming that the patch was built and read back.
5. In MO2, enable the **Craft Output** mod and its **CraftPatch.esp** plugin. Refresh MO2's lists if the new output is not yet visible.
6. Place `CraftPatch.esp` after the recipe sources and other recipe patches whose changes you want Craft to override.
7. Launch Skyrim through MO2. At the affected crafting station, check the recipe with and without the chosen materials. Meet any existing perk, quest, and other source requirements too.

The output is `<MO2 instance>\mods\Craft Output\CraftPatch.esp`. Craft writes the file but leaves activation and plugin ordering to you.

## Find the recipes you want

**Bulk decisions** groups recipes by their origin or source mod. Select several mods with **Ctrl** or **Shift**. **All mods** clears the selection; with no source mods selected, every source mod is included.

The mod grouping identifies where a recipe originates. Its current values come from the last active plugin that overrides it. Inspect the **Winning plugin** in the recipe details when another patch has changed the recipe.

Use these filters together:

| Control | How to use it |
|---|---|
| **Search recipes** | Search recipe EditorIDs, recipe FormIDs, source mod folders, source plugins, station identifiers, or created-object FormIDs. A displayed recipe label is derived from its EditorID; an item's in-game name may not match. |
| **Crafting station** | Limit the results to one station. Station names may appear as keyword EditorIDs or FormIDs. **No station** means the recipe has no station keyword. |
| **Show** | Review source properties or your saved visibility and ingredient choices. |

The **Show** choices mean:

| Choice | Recipes included |
|---|---|
| **All recipes** | Every recipe matching the other filters. |
| **Source: no conditions** | Recipes with no conditions in the scanned source version. |
| **Source: no ingredients or conditions** | Recipes with neither ingredients nor conditions in the scanned source version. |
| **Changed decisions** | Recipes with a saved visibility change or ingredient edit. |
| **Hidden** | Recipes you have chosen to hide. |
| **Shown by carried ingredients** | Recipes with a saved carry rule. |
| **Ingredient edits** | Recipes with a saved cost change. |

Source filters describe the input recipes before your Craft decisions. They do not evaluate your character, your inventory, or the game's live crafting menu. Craft excludes its own `CraftPatch.esp` from the input scan so previous output does not become the source for the next build.

## Choose the scope of a bulk action

On **Bulk decisions**, actions apply to **every recipe matching the selected mods and filters**, including results you have not scrolled into view. Check the **results** count first. Selecting no source mod includes all mods.

On **Inspect recipes**, select multiple rows with **Ctrl** or **Shift**, then choose **Selected recipes** for the action cards. Choose **All results** to apply an action card to the full filtered result set. Switching into this view selects **Selected recipes** by default; check the scope before each bulk change.

The buttons inside the **Selected recipe** detail pane act on that one recipe. They are separate from the action cards above the list.

Visibility and cost decisions are saved as you apply them. Use **Changed decisions**, **Hidden**, or **Ingredient edits** to review your choices. Filters based on a decision may remove a row from the results as soon as you change that decision.

## Change recipe visibility

| Action | Result |
|---|---|
| **Use original visibility** or **Keep** | Removes Craft's visibility choice. Source visibility requirements continue to apply. Existing cost edits remain. |
| **Hide from crafting** or **Hide** | Makes the recipe unavailable through the generated patch. The item itself is not removed from your game. |
| **Show when ingredients carried…** or **Set visibility…** | Adds a requirement to carry a chosen number of different ingredient types, alongside the source requirements. |

**Keep does not force a recipe to appear.** A source mod may still require a perk, quest stage, item, or other condition. Restoring visibility also does not restore an edited crafting cost.

### Understand carry rules

A carry rule counts **different ingredient types**, with at least one of each counted type in your inventory. It uses the recipe's current ingredient list, including your cost edits.

Suppose a recipe costs four iron ingots, two leather strips, and one linen wrap:

| Rule | Carry requirement |
|---|---|
| **Carry 1 of 3** | At least one iron ingot, leather strip, or linen wrap. |
| **Carry 2 of 3** | At least one of any two of those types. |
| **Carry 3 of 3** | At least one of each of the three types. |

Four iron ingots count as one type. Even **Carry all** checks one of every type; it does not check that you have the full quantities needed to craft. Making the item still consumes four ingots, two strips, and one wrap, and all source requirements must also be met.

### Set rules for a collection

1. Set the source mods, filters, and action scope.
2. Click **Show when ingredients carried…**.
3. When several recipes are targeted, the dialog groups them by how many different ingredients they use.
4. Choose **Carry n of total** for each group you want to change, or use a quick choice.
5. Review the number of recipes that will change and click **Save visibility**.

**Carry any 1** chooses one type for each eligible group. **Carry all** chooses every type. **Carry up to 2** chooses one for one-type recipes and two for recipes with two or more types.

Groups left on **Choose a number...** keep their existing decisions. **Clear choices** clears the selections in the open dialog; it does not erase saved rules. Recipes with no ingredients are skipped. Add a cost ingredient first if you want to give such a recipe a carry rule.

Some thresholds on recipes with many different ingredients would require too many game conditions. Craft limits generated carry-rule conditions to 512 per recipe and asks you to adjust unsupported choices. Choose a simpler threshold such as one type or all types.

## Edit crafting costs

### Add one ingredient to several recipes

1. Set your filters and scope, then click **Add ingredient to cost…**.
2. Use **Find material…** or enter the ingredient FormID.
3. Choose a quantity from **1 to 9999** and apply the change.

If the ingredient is absent, Craft adds it. If it is already present, Craft sets its quantity to the number you chose; it does not add that number to the previous quantity. This action keeps the saved visibility mode and threshold. Adding a new type also changes the ingredient pool evaluated by an existing carry rule.

### Change a single recipe

1. Select the recipe in **Inspect recipes**.
2. Click **Edit cost…** in **Selected recipe**. Double-clicking a recipe also opens its cost editor.
3. Edit ingredient IDs and quantities. Use a new row to add an ingredient or select a row and delete it to remove one.
4. Click **Save ingredients**.

**Find material…** fills the currently selected existing row, or adds a row when the new row is selected. Check the row before choosing a material so you replace the ingredient you intend to change.

To undo a recipe's cost edits, click **Restore source ingredients**, then **Save ingredients**. The source is the winning recipe from the current scan. If an existing carry rule requires more types than the restored cost contains, first lower the rule or choose **Keep**, then restore the ingredients.

### Find materials and enter IDs

The material picker searches active-profile miscellaneous items and alchemy ingredients by name, EditorID, defining plugin, or FormID. Select a result and use **Use selected material**, or double-click it. The first opening builds a cached material catalog. Only the first 300 matches are displayed; narrow the search for more specific results.

Other item types can be entered manually. Use an ID in the format `XXXXXX:Plugin.esp`: the item's local hexadecimal ID followed by its defining plugin, rather than a load-order-dependent console ID. The plugin must be active. Copy a known valid ID and check the result in game; manual entry is not a full item-record lookup.

## Build and manage the patch

**Build patch** includes all active saved changes, regardless of the current filters or selected rows. It creates overrides from the scanned winning recipes. Hidden recipes are marked deleted in the output plugin; the source plugins are not edited. Other selected recipes receive their cost edits and carry conditions.

Craft checks that the profile has not changed since scanning. It writes a temporary plugin, reopens it to check record counts, hide flags, ingredient-entry counts, and carry-condition counts, then replaces the previous `CraftPatch.esp`. This verifies the generated structure; check the actual menu behaviour in Skyrim too.

Rebuild whenever you change decisions. After adding, removing, updating, or reordering source plugins, click **Rescan MO2**, review the recipes and warnings, then rebuild. Rescanning reloads the input and retains your saved decisions.

**A failed build leaves the previous output in place.** If that old patch is enabled, Skyrim will continue using it. Resolve the error and build again, or disable the old output while working.

If Craft says **No recipe decisions to patch**, there are no active changes to write. An earlier patch is not automatically cleared. Disable or remove the previous output if you want the game to use the source recipes again.

## Profiles and load order

Craft uses the profile selected in MO2. It has no separate in-app profile selector. After changing the MO2 profile, use **Rescan MO2** and check the status line before editing or building.

Each app installation stores **one decision set**, rather than a separate set for each profile. Decisions are matched by recipe ID. The same recipe ID in another profile can receive an existing decision; inactive recipe IDs are retained and reported as warnings.

`Craft Output` is also one physical mod folder per MO2 instance. Rebuilding replaces its plugin for every profile that enables that output. Back up decision sets when maintaining different choices, and rebuild for the profile you intend to play. Separate copies of the app can keep separate decisions, but still target the same output folder when pointed at the same instance.

Place the plugin after the recipe edits you want it to override. A later plugin can replace a hidden flag, ingredient list, or condition. Rebuild against the final source load order when you add another recipe patch. Runtime crafting systems may apply further changes that are not represented in Craft's plugin scan.

## Back up, update, or remove Craft

Close Craft before copying or replacing its files. Back up `app\decisions.json` before updating, moving between decision sets, or resetting your choices. Preserve this file when extracting a new release into an existing installation.

| File or folder | Purpose |
|---|---|
| `app\decisions.json` | Saved visibility rules and ingredient edits. Created when you save decisions. |
| `app\craft-theme.txt` | Saved colour theme. |
| `app\profile-cache.json` | Cached MO2 profile resolution. |
| `app\recipe-cache.json` | Cached input recipe catalog. |
| `app\material-cache.json` | Cached material search catalog. |
| `<MO2 instance>\mods\Craft Output\CraftPatch.esp` | Generated plugin used by Skyrim when enabled. |
| `%LOCALAPPDATA%\Nymphs -Craft\launcher-diagnostic.log` | Launcher diagnostics. |
| `%LOCALAPPDATA%\Nymphs -Craft\launch-diagnostic.log` | App launch diagnostics. |

Colour swatches at the top right choose Grey, Pine, Sky, Pink, or Brown for the app's appearance. Grey is the default; themes do not affect recipes.

To reset every decision, close Craft, move `app\decisions.json` to a backup location, and reopen Craft. Disable the old output in MO2 too; resetting the app's decisions does not clear an existing patch.

To turn off Craft's changes, disable `CraftPatch.esp` or **Craft Output** in MO2. To remove the tool, close it, remove its MO2 executable entry and app folder, and remove the output if you no longer need it. Keep your decisions backup if you may return later.

## Command-line diagnostics

For troubleshooting and automation, the managed application accepts the following arguments. Use `--instance "C:\path\to\your\MO2 instance"` to specify the instance explicitly. These operations use the selected MO2 profile and the same saved decisions as the desktop interface.

| Argument | Operation |
|---|---|
| `--summary` | Scans the profile and reports recipe totals, decision totals, and warnings as JSON. |
| `--scan` | Scans and reports the full recipe catalog as JSON. |
| `--verify` | Reports the existing output plugin's masters and recipe structure as JSON. It does not build a new patch. |
| `--build` | Scans and builds the output from active saved decisions. This replaces the output after successful validation. |
| `--check-materials` | Reads the material catalog and reports a count and a sample material as JSON. |
| `--check-ui` | Constructs the desktop window to check UI setup, then exits. It does not run an in-game test. |

When collecting JSON output, invoke the application executable inside `app` directly; the top-level native launcher does not forward its child's console output. Scans and material checks can update cache files, and launches write diagnostic logs. Use **Rescan MO2** in the interface after changing source plugins or profiles.

## Troubleshooting

| Symptom | What to check |
|---|---|
| **Could not find the MO2 instance** | Keep the complete app inside the instance's `tools` folder, or set `--instance` to the folder containing `ModOrganizer.ini`, `profiles`, and `mods`. |
| **Skyrim Data folder not found** or missing **gamePath** | Correct the game path in MO2 and confirm that Skyrim's `Data` folder exists. |
| The app does not open | Extract the whole release again, including `app`. Check both launch logs listed above. A framework-dependent development build differs from the self-contained player release. |
| An expected recipe is missing | Check the active profile, enabled source plugin, selected source mods, search, station, and Show filter. Click **All mods**, clear the search, choose **All stations** and **All recipes**, then rescan. Review scan warnings. |
| A plugin is skipped or missing | Confirm the file is present and enabled in the active profile. Resolve the reported plugin problem and rescan. An incomplete scan may omit recipes or winners. |
| **Saved decision has no active recipe** | The recipe is no longer in this profile or its ID changed. The saved decision is retained. Restore the relevant source if intended, or keep the backup while revising your choices. |
| **The MO2 profile changed since the scan** | Click **Rescan MO2**, review the new inputs, then build again. |
| A carry rule cannot be saved or built | Ensure the recipe has enough different ingredients for the threshold. Lower an unsupported threshold, or use one type or all types for a large recipe. |
| Ingredient changes are refused | Use a local hexadecimal FormID with its defining plugin, an active ingredient plugin, and quantities from 1 to 9999. Lower a carry threshold before removing ingredient types it needs. |
| The recipe appears but cannot be crafted | Carry rules check ingredient types. You still need the complete crafting quantities and all source requirements. |
| A recipe remains hidden after **Keep** | Source conditions may still hide it. Rebuild after changing the decision and check that the updated patch wins. |
| Skyrim shows the old behaviour | Confirm the build succeeded, the intended profile is active, the mod and plugin are enabled, and no later plugin overrides it. Restart Skyrim after changing the output. |
| **No recipe decisions to patch** | Make at least one active change to build. If returning everything to source behaviour, disable the old output; it is not automatically emptied. |

For a useful bug report, include the Craft release you installed, the error text, the recipe ID and source/winning plugins when relevant, and the steps that produced the problem. Include the relevant launch-log excerpt for startup issues. Review logs for personal paths before sharing them.
