# Contributing to CleanLink Rules

Thank you for helping keep CleanLink's tracking-parameter database up to date!

## Quick Start

1. Fork this repository.
2. Edit `rules.json` — either add a new domain rule block or update `global_rules`.
3. **Bump the `version` field** (semver, e.g. `2.0.1` → `2.0.2`). PRs that don't bump the version will be rejected by CI.
4. Open a PR. CI will run Zod validation and regex unit tests automatically.

## Rule Format

### Global Rule

A global rule is applied to **every URL** regardless of domain. Use these only for parameters that are universally tracking-related (e.g. `utm_*`, `gclid`).

```json
{
  "name": "My Tracker",
  "regex": "[?&]mytracker=[^&]*",
  "comment": "Explanation of what this parameter does"
}
```

### Domain Rule

A domain rule is applied only to URLs whose hostname matches `domain_pattern`.

```json
{
  "domain_pattern": "^(?:.*\\.)?example\\.(?:com|co\\.uk)$",
  "rules": [
    { "name": "Example ref", "regex": "[?&]ref=[^&]*" }
  ]
}
```

## Regex Guidelines

| Do | Don't |
|:---|:------|
| Use `[?&]param=[^&]*` to match a query parameter | Use `.*` — it's too broad |
| Anchor domain patterns with `^` and `$` | Leave domain patterns unanchored |
| Support TLD variants in one pattern | Create separate entries per TLD |
| Test your regex on https://regex101.com (JavaScript flavour) | Submit untested regexes |

## Domain Pattern Formats

| Format | Example | Matches |
|:---|:---|:---|
| Subdomain wildcard | `^(?:.*\\.)?amazon\\.com$` | `amazon.com`, `smile.amazon.com` |
| TLD variants | `^(?:.*\\.)?ebay\\.(?:com\|co\\.uk\|de)$` | `ebay.com`, `ebay.co.uk`, `ebay.de` |
| Exact domain | `^t\\.co$` | `t.co` only |

## CI Gates

Your PR must pass:

1. **Zod validation** — `npm run validate-rules`
2. **Regex unit tests** — all existing Jest tests must stay green
3. **Version bump** — `version` in `rules.json` must be higher than on `main`
4. **Size check** — `rules.json` must be under 500KB

## License

Rule patterns are factual descriptions of URL query-parameter structures and are not subject to copyright. The `rules.json` file is released under **CC0 (No Rights Reserved)**.
