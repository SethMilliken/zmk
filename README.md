# Display Improvements for the Corne-ish Zen Keyboard

## ABOUT THIS BRANCH

A collection of patches to the ZMK codebase to improve various aspects of the Corne-ish Zen e-ink displays.

This branch tracks [ZMK `v0.3`](https://github.com/zmkfirmware/zmk/tree/v0.3), and is not likely to change unless there are new Zen-specific patches. If you have any problems with this branch, feel free to ping me on the [ZMK Discord Server](https://zmk.dev/community/discord/invite) (`Araxia#7830`).

If you want to track the ZMK development release version (`main`), use branch [`zen-display-patches`](https://github.com/SethMilliken/zmk/tree/zen-display-patches) instead.

Use these board names in your `build.yaml`:

For > v1 (R3 of the GB):
```
- board: corneish_zen_left
- board: corneish_zen_right
```

For v1 (R1/R2 of the GB):
```
- board: corneish_zen_left@1
- board: corneish_zen_right@1
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
      revision: zen-display-patches-0.3  # custom branch name
      import: app/west.yml
  self:
    path: config
```
...or alternatively, you can cherry-pick specific commits noted below to your own fork of ZMK.


## Details of Improvements

Below are the details of improvements, some are always in effect and some require setting corresponding config to be enabled, as detailed below.

1. Avoid unnecessarily refreshing the battery level widget (reduce ghosting)
2. Tweak battery level thresholds so the battery levels are more "accurate"
3. Add option to hide layer changes that are momentary (using `&mo`, `&lt` etc.) from the layer widget (reduces the number of partial refreshes)
    - Add `CONFIG_ZMK_DISPLAY_HIDE_MOMENTARY_LAYERS=y` to your `corneish_zen.conf` file to enable
4. Add a periodic full display refresh (to clear up ghosting) using zmkfirmware/zmk#969
    - Add `CONFIG_ZMK_DISPLAY_FULL_REFRESH_PERIOD=N` to your `corneish_zen.conf` file to refresh every `N` seconds
5. Add option to invert displays to white-on-black instead of black-on-white
    - Add `CONFIG_IL0323_INVERT=y` to your `corneish_zen.conf` file to enable
6. Add option to hide the "LAYER" heading in the layer widget and realign all widgets
    - Add `CONFIG_CUSTOM_WIDGET_LAYER_STATUS_HIDE_HEADING=y` to your `corneish_zen.conf` file to enable
7. Add options to select different logo images to replace the "Corne-ish Zen" logo on the right half, from @manna-harbour:
    - `CONFIG_CUSTOM_WIDGET_LOGO_IMAGE_ZMK=y` for ZMK logo instead of Zen
    - `CONFIG_CUSTOM_WIDGET_LOGO_IMAGE_LPKB=y` for LowProKB logo
    - `CONFIG_CUSTOM_WIDGET_LOGO_IMAGE_MIRYOKU=y` for Miryoku logo
8. Add option to enable @aumuell's driver tweak to reduce ghosting for partial refreshes
    - This causes a different fading pattern on partial refreshes, with vertical/horizontal banding rather than full screen. However it can make the fading stronger for some screens
    - Add `CONFIG_IL0323_ALTERNATIVE_REFRESH=y` to your `corneish_zen.conf` to enable.


## Updates

- 2026-07-02: Rebased onto tag `v0.3`; I had originally just used the exact state of the branch when @caksoylar transferred maintenance (`9e36ebd5`), but that was well behind `v0.3`.
- 2026-06-08: Added branch `zen-display-patches-0.3` for staying pinned to ZMK version `v0.3`.

## History

Originally compiled and maintained by [@caksoylar](https://github.com/caksoylar) and described [here](https://gist.github.com/caksoylar/c411313990978e1903c244f03039187a). [@SethMilliken](https://github.com/SethMilliken/) took over maintenance 2025-12.
