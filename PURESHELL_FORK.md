# Changes in this fork

This is a fork of [TerminalStudio/xterm.dart](https://github.com/TerminalStudio/xterm.dart)
for [PureShell](https://github.com/janos-raul/pure-shell). It is based on the
upstream `v4.0.0` tag (identical to `xterm 4.0.0` on pub.dev) and keeps the
MIT license and all copyright notices unchanged (see `LICENSE`).

The `pureshell` branch has these changes on top of `v4.0.0`; nothing else.

1. **Bold is bright (optional).** `TerminalTheme.boldIsBright` (default
   `false`, so existing themes behave exactly as before). When true, bold text
   in one of the 8 normal ANSI colors (30-37, or 256-color index 0-7) is drawn
   in the bright variant (8-15). Default foreground, colors 8-255, RGB colors
   and inverse-video cells are not changed.
   Files: `lib/src/ui/terminal_theme.dart`, `lib/src/ui/painter.dart`.
2. **Palette index 15 is `brightWhite`.** It returned `white`, so `ESC[97m` and
   256-color index 15 drew the normal white.
   File: `lib/src/ui/palette_builder.dart`.
3. **Dev tooling only.** Dropped the abandoned `dart_code_metrics` dev
   dependency and its analyzer plugin entry, which cannot be resolved on current
   Dart SDKs (so `flutter pub get` and the tests could not run). Packages that
   depend on xterm are not affected.
   Files: `pubspec.yaml`, `analysis_options.yaml`.

Tests for 1 and 2: `test/src/ui/bold_bright_test.dart`.

## Known upstream behavior, not changed here

- SGR 22 ("normal intensity") only clears faint, not bold (SGR 21 clears bold).
  Programs that end bold with `ESC[22m` leave the text bold.
- Two golden-image tests (`TerminalView.textScaler ...`) differ by 0.06% and
  0.13% of pixels with the fonts on some machines; this is unrelated to colors.
