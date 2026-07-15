# raven-ini

[![CI](https://github.com/martian56/raven-ini/actions/workflows/ci.yml/badge.svg)](https://github.com/martian56/raven-ini/actions/workflows/ci.yml)

Reads INI-style config: `[sections]`, `key = value`, and `;` or `#` comments.
Keys before the first section go in the global section, which you read with an
empty name. No dependencies.

## Install

```toml
[dependencies]
"github.com/martian56/raven-ini" = "v0.1.0"
```

## Usage

```rust
import "github.com/martian56/raven-ini" { parse }

let text = "[server]\nhost = 0.0.0.0\nport = 8080\n"
match parse(text) {
    Ok(cfg) -> {
        let host = cfg.get_or("server", "host", "localhost")
        let port = cfg.get_or("server", "port", "80")
    },
    Err(e) -> print(e.to_string()),   // "ini:3: expected key = value"
}
```

A key that appears before any `[section]` lives in the global section, which you
read with `""` as the section name:

```rust
cfg.get_or("", "title", "")
```

## What's there

`get(section, key)` returns an `Option<String>`, and `get_or(section, key,
fallback)` falls back when it's missing. `has(section, key)` is the presence
check. `section(name)` hands you a plain map of one section's keys, and
`sections()` lists the section names in order.

Values are trimmed but otherwise taken as written, and comments go on their own
line.

## License

MIT
