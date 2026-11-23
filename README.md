# pharo-agent-study

Evref-BL's study of how agents use the Pharo agent tools, run by
[pharo-agent-telemetry](https://github.com/Evref-BL/pharo-agent-telemetry),
whose README says how a study works and how its collector is deployed.

Participants read [docs/participants.md](docs/participants.md), then:

```sh
npx @evref-bl/pharo-agent-study preview
npx @evref-bl/pharo-agent-study join
npx @evref-bl/pharo-agent-study status
npx @evref-bl/pharo-agent-study leave
```

This repository is the study's data, and what pharo-agent-telemetry makes of
it:

- [study.json](study.json): who runs it, its collector, and what the page
  says of its purpose, retention and authority;
- [allowlist.json](allowlist.json) and [catalog.json](catalog.json): what is
  kept;
- [rules/](rules): a fixture per rule;
- generated, never edited: `telemetry-transform.mjs`, `collector/` and
  `docs/participants.md`.

## Working on it

```sh
npm install
npm run generate   # after changing study.json, the allowlist or the catalog
npm test           # generated files current, rule fixtures through both filters
```

For each release of MCP, PLexus or pharo-image-fs, the catalog is read
again, from checkouts of the releases in use, then generated and signed:

```sh
npx @evref-bl/pharo-agent-telemetry exception-names --mcp <MCP> --cli <PLexus CLI> --out <directory>
npx @evref-bl/pharo-agent-telemetry catalog --mcp <MCP> --plexus <PLexus> --pharo-image-fs <pharo-image-fs> --exceptions <directory>
npm run generate
npx @evref-bl/pharo-agent-telemetry sign-catalog --key <the study's key, outside this repository>
```

`npm publish` refuses while `study.json` lacks any collector fact the page
owes participants. The import job keeps the spans as long as its
`RETENTION_DAYS` says: leave it at its default, 365, which is what
`study.json`'s retention text states.
