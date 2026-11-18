# Dove patch

Branch `dove` is upstream archive 3.6.1 (tag `3.6.1`) with one change, used by
the Dove app as a git submodule through `dependency_overrides`.

Three conditional imports pick the pure-Dart implementation with
`if (dart.library.js_interop)` instead of `if (dart.library.js)`, which is false
under dart2wasm. Without it, a Wasm web build gets stubs that throw
`UnsupportedError` from `ZipDecoder`, `ZLibDecoder` and CRC-64:

- `lib/src/zlib/inflate_buffer.dart`
- `lib/src/zlib_decoder.dart`
- `lib/src/util/crc64.dart`

archive 4.x does not need this. Drop the fork once the app's dependencies that
pin archive 3.x (docx_to_text, spreadsheet_decoder) allow 4.x.
