---
name: make-krate-app
description: Make, change, check and share a Krate app -- one small .krate file that runs natively on Mac, Windows and Linux and opens from a link in any browser. Use when the person asks to build a small app, tool, game or utility "as a Krate app" or "as a .krate", to change an existing .krate, to port a project to Krate, or to publish or share one.
---

# Make a Krate app

A Krate app is one `.krate` file: a WebAssembly component plus a manifest
that declares every permission it needs. It runs natively on macOS, Windows
and Linux with the Krate runtime, and opens from a link in any browser. It can
reach nothing it did not declare: files, network hosts, the camera and the
rest are each asked for, in plain words, before the app opens.

## 0. Two ways to build: on this computer, or through the Krate connector

- **No `krate` here** (Claude chat on the web, desktop or phone, or a computer
  without Krate): use the Krate connector's tools. Call `get_krate_guide` and
  read it before writing any code; write the app's files (`Cargo.toml`,
  `src/lib.rs`, `manifest.toml`, any `assets/`); call `build_app` with them,
  then `build_status` until it is done. If the build fails, it returns the
  check's own words: fix the code and call `build_app` again. When it is done
  you get a link that opens the app on any phone, computer or browser -- give
  it to the person. The first build asks them to sign in to Krate once.
- **`krate` is installed** (`krate --version` answers): follow the steps below
  on this computer. To install it: https://krate.tech/download/ (macOS,
  Windows and Linux); do not install it for them.

Apps are written in Rust and built for `wasm32-wasip1`; `krate doctor` says
what is missing and `rustup target add wasm32-wasip1` adds the target.

## 1. Learn the real API before writing code

Never guess Krate's interfaces. In the app's folder run:

```sh
krate authoring-context . -o KRATE_AUTHORING.md
```

and read `KRATE_AUTHORING.md`: the SDK's functions, the permission catalog,
the `no_std` rules a Krate app follows, and an index of working example apps.
It is generated from the same sources the runtime builds against, so it is
current. `krate krate-mode` prints a shorter version for a quick answer.

## 2. Start from something that works

- A new app: start from the template
  (`git clone https://github.com/incyashraj/krate-app-template my-app`), a
  small working app whose layout (`Cargo.toml`, `src/lib.rs`,
  `manifest.toml`) every Krate app shares.
- An existing `.krate`: it carries its own source. `krate revise app.krate
  "the change"` edits that source with an AI and replaces the file only with
  a build that passed every check.
- An existing project in another language or format: `krate report <dir>`
  says honestly whether and how it can become a Krate app; `krate port` does
  the move where it can (beta).

## 3. Check after every change

```sh
krate check-app .
```

It builds the app, confirms it imports only Krate's interfaces, runs it once,
and on failure names the stage and the fix. Do not call an app done until it
prints `OK`. For a windowed app, `krate check-app . --shoot first-frame.png`
paints its first frame so you can look at it.

## 4. Declare only what the app needs

Every capability goes in `manifest.toml` with a one-line rationale the person
reads before the app opens. Ask for the narrowest one: a file the person picks
rather than a folder, one host rather than the network. If a request cannot
work in a sandbox (reading other apps, scraping a mailbox), say so plainly and
offer the nearest thing that can.

## 5. Package, run and share

```sh
krate pack target/wasm32-wasip1/release/<name>.wasm --manifest manifest.toml -o <name>.krate --sign
krate run <name>.krate
krate publish <name>.krate
```

`--sign` signs it with the person's own publisher key, made once and kept on
their computer, so the app keeps its data through every rebuild.
`krate publish` prints a link: whoever opens it gets the app in their
browser, on a phone or a computer, with nothing to install. Publishing again
is the app's next version on the same link.

## Show it working

`krate run <name>.krate` opens the app in Krate's sandbox, asking for each
permission in plain words; `krate inspect <name>.krate` says what a bundle is
and asks for without running it. To let Claude Desktop or Cursor build Krate
apps too, `krate connect` sets that up (it asks before changing anything).

## Be straight about limits

Krate is young: Rust is the only SDK language, Windows builds of the runtime
are not code-signed yet, and some abilities (USB, printing, listening on a
port) are still being built. Say so when a request depends on one.
