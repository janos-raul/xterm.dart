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
3. **SGR 22 clears bold and faint.** Upstream only cleared faint, so a
   program that ended bold with `ESC[22m` left the text bold (and, with
   `boldIsBright`, bright). "Normal intensity" now clears both, as standard.
   File: `lib/src/core/escape/parser.dart`.
4. **Dev tooling only.** Dropped the abandoned `dart_code_metrics` dev
   dependency and its analyzer plugin entry, which cannot be resolved on current
   Dart SDKs (so `flutter pub get` and the tests could not run). Packages that
   depend on xterm are not affected.
   Files: `pubspec.yaml`, `analysis_options.yaml`.

Tests: `test/src/ui/bold_bright_test.dart` (1, 2, and 3 with `boldIsBright`),
`test/src/core/escape/sgr_intensity_test.dart` (3).

## Known upstream behavior, not changed here

- SGR 21 clears bold here (as upstream), which is what some terminals do. Others
  read 21 as double underline, so terminals disagree; we left it as it is.
- Two golden-image tests (`TerminalView.textScaler ...`) differ by 0.06% and
  0.13% of pixels with the fonts on some machines; this is unrelated to colors.
