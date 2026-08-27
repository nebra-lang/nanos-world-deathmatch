# nanos-world-deathmatch

A [Nebra](https://github.com/nebra-lang/nebra) port of the official
[nanos world deathmatch](https://github.com/nanos-world/deathmatch) gamemode.

It exists as a worked example: a complete, playable gamemode rather than a snippet, showing what a
real nanos world package looks like when it is written in Nebra instead of plain Lua.

## What it demonstrates

**Execution sides.** `[sides]` in `nebra.toml` maps each folder to where it runs:

```toml
[sides]
"src/Client/**" = ["client", "shared"]
"src/Server/**" = ["server", "shared"]
"src/Shared/**" = ["shared"]
```

Calling a server-only API from a client file is a compile error, not a runtime nil. See
[Sides](https://nebra-lang.github.io/docs/advanced/sides).

**Typed game APIs.** `nanos-world-types` is pulled in as a dependency, so every nanos world class,
struct and enum is typed and the event annotations are available.

**Host-specific codegen.** nanos world loads scripts through `Package.Require` rather than plain
`require`, and expects `.lua` on the path. Nebra emits exactly that without any change to the
source:

```toml
[code]
import_statement = 'Package.Require(%s)'
import_extension = ".lua"
index_base = 1
```

**Non-code assets.** The `[assets]` section copies the package manifest and the NUI folder into the
build output, so `nebra build` produces a directory the game can load as-is.

## Running it

```bash
nebra install
nebra build
```

Copy the resulting output folder into your nanos world server's `Packages/` directory and start it
as the gamemode.

While developing, `nebra watch` rebuilds on every save.

## Layout

```
src/
├── Client/         client-side scripts plus the NUI (index.html, style.css, index.js)
├── Server/         server-side game logic
└── Shared/         code compiled into both sides
types/              project-local declarations, loaded via `globals`
Package.toml        the nanos world package manifest
```

## Related

- [Nebra documentation](https://nebra-lang.github.io)
- [nanos-world-types](https://github.com/nebra-lang/nanos-world-types) - the type declarations and
  event annotations this package depends on
- [Configuration reference](https://nebra-lang.github.io/docs/advanced/configuration) - every
  `nebra.toml` key used above
