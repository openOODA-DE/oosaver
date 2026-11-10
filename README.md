# oosaver

**oosaver** is the openOODA-DE (L4) screensaver, idle-daemon and session-lock
engine: a capability-secured, multi-call binary shipping **51 ported
16-bit ASCII/demoscene scenes** on a zero-wakeup Wayland idle reactor.

```
./bin/oosaver about          # six-field identity & theme card
./bin/oosaver list           # all 51 scenes
./bin/oosaver preview matrix --frames 4 --cols 80 --rows 24
```

## Personas (multi-call)

One binary, four entry names — resolved from `argv[0]` or `--persona`:

| persona | role |
| --- | --- |
| `oosaver` | canonical CLI: `list`, `preview`, `daemon`, `status`, `config`, `doctor`, `about` |
| `idlescreen` | compatibility persona (same surface, legacy name) |
| `ooidle` | idle-control client persona (`status`, `inhibit`, `uninhibit`) |
| `oolock` | session lock: `lock` → saver start, `unlock` → saver stop over the control socket |

`bin/idlescreen`, `bin/ooidle`, `bin/oolock` are hardlinks to `bin/oosaver`.

## Theming & RGB

- `ootheme` wired: **25 presets / 34 semantic tokens**; resolution order
  `--theme` > `OOTHEME` > current-theme file > `OODA_THEME` > legacy file;
  unknown names fall back to `ooda-dark`. `NO_COLOR`, `OO_NO_COLOR`,
  `OODA_NO_COLOR` suppress colour.
- `--rgb` or `OODA_RGB=1` best-effort syncs accent colours to `oorgb`
  peripherals (fail-soft, never required).

## Daemon & lock

Per-user Unix socket `$XDG_RUNTIME_DIR/oosaver/control.sock` (mode `0600`,
no TCP). Verbs: `start`, `stop`, `toggle`, `set`, `timeout`, `inhibit`,
`uninhibit`, `status`, `scene`, `sleep`, `quit`, `shutdown`.
Exit codes: `78` config/policy, `69` display unavailable, `0` clean,
`2` fail-closed refusal.

## Build & verify

```
make build && make test
make portable line-cap file-law academy density vocab shape
make check parity det
```

`make parity` replays every one of the 51 scenes frame-by-frame through
`PortSession` and diffs against `tests/qa/golden/`; `make det` requires two
byte-identical builds. Service units for systemd/dinit/OpenRC/runit/s6 and
the man page live under `cli/assets/`; `cli/install.sh` installs them.

Repository: <https://github.com/openOODA-DE/oosaver>
