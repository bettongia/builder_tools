A small set of utilities for Dart code-generation scripts that need to load
source data from a local file cache or a remote URL.

## Features

- **`loadData`** — transparent cache helper: serves from a local file when it
  exists, otherwise fetches from a URL and writes the result to disk.
- **`loadFromLocalFile`** — read a local file as a `String`.
- **`loadFromUrl`** — fetch a URL, optionally decompress a ZIP archive entry,
  and persist the result to a local cache file.
- **`CompressionType`** — `none` (default) or `zip`.
- **`dartfmt`** — a pre-configured `DartFormatter` instance (latest language
  version) ready to format generated source code.

## Getting started

Add the dependency to your `pubspec.yaml` (typically under `dev_dependencies`
for build / codegen scripts):

```yaml
dev_dependencies:
  betto_builder_tools: ^0.1.0
```

## Usage

### Load from a cache or remote URL

```dart
import 'package:betto_builder_tools/betto_builder_tools.dart';

Future<void> main() async {
  // Fetches from the URL on first run; uses the local file on subsequent runs.
  final data = await loadData(
    'data/schema.json',
    'https://example.com/schema.json',
  );

  // ZIP archive — extract a specific entry
  final csv = await loadData(
    'data/records.csv',
    'https://example.com/records.zip',
    compression: CompressionType.zip,
    archiveFilePath: 'export/records.csv',
  );

  // Force re-fetch even if the local file exists
  final fresh = await loadData(
    'data/schema.json',
    'https://example.com/schema.json',
    force: true,
  );
}
```

### Format generated Dart source

```dart
import 'package:betto_builder_tools/betto_builder_tools.dart';

final source = dartfmt.format(generatedCode);
```
