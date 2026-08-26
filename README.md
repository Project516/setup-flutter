# setup-flutter

One composite action for the three-step block every Flutter CI job repeats:
install Flutter, restore the pub cache, run `flutter pub get`. Wraps
[subosito/flutter-action](https://github.com/subosito/flutter-action) and
[actions/cache](https://github.com/actions/cache). The Flutter SDK cache is keyed
on `pubspec.yaml`, while the pub package cache is keyed on both `pubspec.yaml`
and `pubspec.lock`.

## Usage

```yaml
steps:
  - uses: actions/checkout@v7

  - name: Set up Flutter
    uses: Project516/setup-flutter@v1
    with:
      flutter-version: 3.44.4
```

Omit `flutter-version` to track a channel instead (default `stable`):

```yaml
  - uses: Project516/setup-flutter@v1
    with:
      channel: beta
```

## Inputs

| Input | Default | Description |
|---|---|---|
| `flutter-version` | none | Exact Flutter version to install. Wins over `channel` when set. |
| `channel` | `stable` | Channel to track when no exact version is given. |

Run it after checkout and before any `flutter` command. It replaces roughly
20 lines per job; we were carrying nine copies before extracting this.

## Caching

The pub cache step snapshots the directory `flutter pub get` writes to on
the current runner, resolved from the `PUB_CACHE` env var that
[subosito/flutter-action](https://github.com/subosito/flutter-action)
exports for the job. That path is `~/.pub-cache` on Linux and macOS and the
`LOCALAPPDATA` Pub Cache on Windows, so the same workflow caches the right
directory on every runner OS. Leave the key (`${{ runner.os }}-pub-...`) as
is for cross-OS safety: a cache built on Windows is not reused on Ubuntu,
and vice versa.

## License

AGPL-3.0
