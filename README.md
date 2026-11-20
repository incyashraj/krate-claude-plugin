# Krate for Claude Code

Make, check and share Krate apps by asking Claude. A Krate app is one small
`.krate` file that runs natively on Mac, Windows and Linux, opens from a link
in any browser, and can reach only what it declares.

## Install

Install Krate once, from https://krate.tech/download/ (macOS, Windows and Linux).

Then, in Claude Code:

```
/plugin install krate --marketplace incyashraj/krate-claude-plugin
```

or in a terminal:

```sh
claude plugin marketplace add incyashraj/krate-claude-plugin
claude plugin install krate@krate
```

## What it adds

The make-krate-app skill: Claude reads Krate's real API
(`krate authoring-context`), starts from a working template, checks every
change with `krate check-app`, declares only the permissions the app needs,
and packages, signs and publishes it as one link.

Ask something like: "Make me a habit tracker as a Krate app and give me a link."

Docs: https://krate.tech/docs/ · Issues: https://github.com/incyashraj/krate/issues

Krate itself: https://github.com/incyashraj/krate
