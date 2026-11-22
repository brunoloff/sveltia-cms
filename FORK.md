# brunoloff/sveltia-cms

A personal fork of [Sveltia CMS](https://github.com/sveltia/sveltia-cms) for editor features used on my websites. Upstream does not accept pull requests beyond translations and trivial fixes (see `CONTRIBUTING.md`), so this fork is kept easy to rebase rather than aimed at merging back. Ideas worth sharing go upstream as issues.

## Branches

- `main`: an exact mirror of upstream `main`. Never commit here.
- `feature/<name>`: one feature per branch, started from an upstream release tag, small and self-contained so it rebases cleanly.
- `brunoloff`: the integration branch websites build from. It is the latest upstream release tag with the feature branches merged in.

## Updating from upstream

```sh
git fetch upstream --tags
git switch main && git merge --ff-only upstream/main && git push origin main
# Rebase each feature onto the new release, then rebuild the integration branch:
git switch feature/<name> && git rebase --onto vX.Y.Z <old-base-tag> && git push -f origin feature/<name>
git switch brunoloff && git reset --hard vX.Y.Z && git merge --no-ff feature/<name> ... && git push -f origin brunoloff
```

Pushing to `upstream` is disabled in this checkout (`git remote set-url --push upstream DISABLED…`).

## Building

`pnpm install && pnpm build` writes the npm package to `package/`. Websites bundle it themselves; see `scripts/sync-sveltia.sh` in [brunoloff.github.io](https://github.com/brunoloff/brunoloff.github.io).

Before merging a feature, run the upstream checks that apply: `pnpm test:unit`, `pnpm check:eslint`, `pnpm check:svelte`.
