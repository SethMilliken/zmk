# Display Improvements for the Corne-ish Zen Keyboard

## ABOUT THIS BRANCH

A collection of patches to the ZMK codebase to improve various aspects of the Corne-ish Zen e-ink displays.

This branch tracks [ZMK `main`](https://github.com/zmkfirmware/zmk/tree/main), but not on a specific schedule. I'll try to update it for any significant milestones. If you have any problems with this branch, feel free to ping me on the [ZMK Discord Server](https://zmk.dev/community/discord/invite) (`Araxia#7830`).

If you want to stay pinned to the current release version instead of tracking `main`, use branch [`zen-display-patches-0.3`](https://github.com/SethMilliken/zmk/tree/zen-display-patches-0.3) instead.

Note: As of 2026-02-15, you'll need to use these board names in your `build.yaml`:

For > v1 (R3 of the GB):
```
- board: corneish_zen_left//zmk
- board: corneish_zen_right//zmk
```

For v1 (R1/R2 of the GB):
```
- board: corneish_zen_left@1//zmk
- board: corneish_zen_right@1//zmk
```


## Getting the changes

You can test out below changes using your Zen config repo by modifying your `config/west.yml` file, following [ZMK instructions](https://zmk.dev/docs/features/beta-testing):
```yaml
manifest:
  remotes:
    - name: SethMilliken
      url-base: https://github.com/SethMilliken
  projects:
    - name: zmk
      remote: SethMilliken
      revision: zen-display-patches  # custom branch name
      import: app/west.yml
  self:
    path: config
```
...or alternatively, you can cherry-pick specific commits noted below to your own fork of ZMK (n.b. the most recent SHAs of the listed commits will be different).


## Details of Improvements

Below are the details of improvements, some are always in effect and some require setting corresponding config to be enabled, as detailed below.

1. Avoid unnecessarily refreshing the battery level widget (reduce ghosting)
    - https://github.com/SethMilliken/zmk/commit/e1763b0db82540c8d5099babecb7d1d23c567071
2. Tweak battery level thresholds so the battery levels are more "accurate"
    - https://github.com/SethMilliken/zmk/commit/940fd4de4d68446e7ac8ecbbb2ed3c8f973b15ce
3. Add option to hide layer changes that are momentary (using `&mo`, `&lt` etc.) from the layer widget (reduces the number of partial refreshes)
    - https://github.com/SethMilliken/zmk/commit/ae968bd0fe34c52c5872f6190144c670cf498a95
    - Add `CONFIG_ZMK_DISPLAY_HIDE_MOMENTARY_LAYERS=y` to your `corneish_zen.conf` file to enable
4. Add a periodic full display refresh (to clear up ghosting) using zmkfirmware/zmk#969
    - https://github.com/SethMilliken/zmk/commit/0d75117e97688f1eb7b070fa49180067d7e82d2e
    - Add `CONFIG_ZMK_DISPLAY_FULL_REFRESH_PERIOD=N` to your `corneish_zen.conf` file to refresh every `N` seconds
5. Add option to invert displays to white-on-black instead of black-on-white
    - https://github.com/SethMilliken/zmk/commit/2d68f8e02c44144e60eb8dbd6ef9aaef275915f5
    - Add `CONFIG_IL0323_INVERT=y` to your `corneish_zen.conf` file to enable
6. Add option to hide the "LAYER" heading in the layer widget and realign all widgets
    - https://github.com/SethMilliken/zmk/commit/0a81b1832fd113a9ddd39ab776ae54720937dd9d
    - Add `CONFIG_CUSTOM_WIDGET_LAYER_STATUS_HIDE_HEADING=y` to your `corneish_zen.conf` file to enable
7. Add options to select different logo images to replace the "Corne-ish Zen" logo on the right half, from @manna-harbour:
    - https://github.com/SethMilliken/zmk/commit/1160c77055776181ce5831067116123c2dc32101
    - `CONFIG_CUSTOM_WIDGET_LOGO_IMAGE_ZMK=y` for ZMK logo instead of Zen
    - `CONFIG_CUSTOM_WIDGET_LOGO_IMAGE_LPKB=y` for LowProKB logo
    - `CONFIG_CUSTOM_WIDGET_LOGO_IMAGE_MIRYOKU=y` for Miryoku logo
8. Add option to enable @aumuell's driver tweak to reduce ghosting for partial refreshes
    - https://github.com/SethMilliken/zmk/commit/02972a3cfb1c55689d4f6455d3fa5a01ca94f9b2
    - This causes a different fading pattern on partial refreshes, with vertical/horizontal banding rather than full screen. However it can make the fading stronger for some screens
    - Add `CONFIG_IL0323_ALTERNATIVE_REFRESH=y` to your `corneish_zen.conf` to enable.


## Updates

- 2026-06-08: Added branch for staying pinned to ZMK `v0.3`.
- 2026-03-21: Updated this `README.md` with details about this branch.
- 2026-03-21: Pulled in upstream `ZMK_BOARD_COMPAT` changes to fix build.


## History

Originally compiled and maintained by [@caksoylar](https://github.com/caksoylar) and described [here](https://gist.github.com/caksoylar/c411313990978e1903c244f03039187a). [@SethMilliken](https://github.com/SethMilliken/) took over maintenance 2025-12.
