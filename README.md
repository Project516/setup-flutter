# setup-flutter

One composite action for the three-step block every Flutter CI job repeats:
install Flutter, restore the pub cache, run `flutter pub get`. Wraps
[subosito/flutter-action](https://github.com/subosito/flutter-action) and
[actions/cache](https://github.com/actions/cache), with both caches keyed on
`pubspec.lock`.

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

## License

AGPL-3.0
