# Jujutsu VCS Plugin for IntelliJ IDEA

Native IntelliJ integration for [Jujutsu (jj)](https://jj-vcs.github.io/jj/), a Git-compatible version control system built around a fundamentally different workflow: your working copy is always a commit.

> **This is the [watbe/jj-idea](https://github.com/watbe/jj-idea) fork** of [kkkev/jj-idea](https://github.com/kkkev/jj-idea).
> See [Maintaining this fork](#maintaining-this-fork) for how to install its builds and how to bring in upstream changes.

![Custom log view with commit graph and tooltip](docs/images/log-light-with-tooltip.png)

## Features

- **Describe-First Workflow** — The Working Copy tool window lets you describe your current work and create new changes with one click. No staging area, no "WIP" commits.
- **Custom Log View** — Visual commit graph with inline change IDs, descriptions, bookmarks, and author info. Filter by text, branch, author, or date range.
- **Change Operations** — Edit, abandon, describe, squash, split, duplicate, rebase and restore changes directly from the log context menu.
- **Operation Previews** — Full dialogs with for rebase, squash and split, with visual destination pickers and live previews.
- **Git Remotes** — Fetch and push to Git remotes without leaving the IDE.
- **File History & Annotations** — Full file history with diff viewer. Line-by-line blame annotations.
- **Multi-Repository Support** — Work with multiple JJ repositories in a single project with a unified log view.
- **Real-Time Status** — Auto-refresh keeps the UI in sync as you edit files.
- **Bookmarks & Tags** — Actions to create, move, advance, track, untrack, delete and push bookmarks, and set and delete tags.
- **Hunk-Level Squash & Split** — Line/hunk granularity for moving changes between commits, plus a live preview panel for the Squash Into dialog.
- **Bookmark Management & Branches Panel** — Interactive bookmark decorations, pending-deletion visibility, distinguishing local from tracked-remote drift, and a dedicated branches panel (the main-toolbar bookmark widget has shipped)
- **Conflicts** — Support for viewing and resolving conflicts in any order.

### Working Copy

The Working Copy panel sits on the left side of the IDE, showing changed files grouped by directory with status coloring. Describe your work and create new changes without leaving your editor.

![Working copy panel](docs/images/working-copy-light.png)

### Log View

The custom log replaces the standard VCS log with a JJ-native view. Hover over any commit for details, or right-click for operations.

![Log with context menu](docs/images/log-light-with-menu.png)

Works in both light and dark themes:

![Log view in dark theme](docs/images/log-dark-with-tooltip.png)

### Rebase Dialog

Visual rebase with source mode selection, searchable destination picker, and a live preview showing the result before you commit to it.

![Rebase dialog with live preview](docs/images/rebase-light.png)

## Installing

### From Jetbrains Marketplace (Recommended)

Search for "Jujutsu VCS Integration" in the Marketplace tab and install. Or install from the [plugin page](https://plugins.jetbrains.com/plugin/30576-jujutsu-vcs-integration). 

### From Custom Repository

1. In IntelliJ IDEA: **Settings → Plugins → ⚙️ → Manage Plugin Repositories**
2. Add: `https://raw.githubusercontent.com/kkkev/jj-idea/master/updatePlugins.xml`
3. Search for "Jujutsu VCS Integration" in the Marketplace tab and install

Future updates will be detected automatically.

### From GitHub Releases

1. Go to [Releases](https://github.com/kkkev/jj-idea/releases)
2. Download the latest `.zip` file
3. In IntelliJ IDEA: **Settings → Plugins → ⚙️ → Install Plugin from Disk**

### Build from Source

```bash
./gradlew buildPlugin
```

Then install from `build/distributions/` via **Settings → Plugins → Install Plugin from Disk**.

## Requirements

- IntelliJ IDEA 2025.2 or later
- [Jujutsu](https://jj-vcs.github.io/jj/latest/install/) (`jj`) version 0.37 or later (0.39 or later recommended) installed, available in PATH (or otherwise configured)

## Getting Started

1. Open a project with a `.jj` directory, or create one with **VCS → Create JJ Repository**
2. The Working Copy panel appears on the left; the Jujutsu log appears in the Version Control tool window
3. Configure settings at **Settings → Version Control → Jujutsu**

## Support

This plugin is free and maintained in my spare time, with a lot of the work done using AI
coding tools. If it saves you time, you can [sponsor me on GitHub](https://github.com/sponsors/kkkev)
to help fund those tools and continued development — entirely optional. Feedback and issues
are just as welcome.

### Sponsors

Thanks to everyone who has sponsored this plugin! Recurring sponsors at $20/month or more
are listed here.

## Roadmap

See **[ROADMAP.md](ROADMAP.md)** for planned features, including hunk-level squash & split, a bookmark widget and branches panel, operation log & undo, and forge integration.

## Documentation

- **[Contributing](contributing.md)** — Architecture, coding standards, testing, and workflow
- **[Agent Instructions](CLAUDE.md)** — Operating rules for AI coding agents working in this repo

## License

[Apache License 2.0](LICENSE)

## Maintaining this fork

This fork keeps a small stack of its own commits on top of upstream's `master`, and publishes
its own releases so they can be installed alongside upstream's update channel. This section is
at the end of the file on purpose: upstream rarely edits here, so rebases rarely conflict on it.

### Installing fork builds

In IntelliJ IDEA: **Settings → Plugins → ⚙️ → Manage Plugin Repositories**, and add
`https://raw.githubusercontent.com/watbe/jj-idea/master/updatePlugins.xml` in place of
upstream's URL. The plugin ID is the same as upstream's, so a fork build replaces the upstream
plugin. The fork's releases are also on [its Releases page](https://github.com/watbe/jj-idea/releases).

### The fork's commits

The commits on top of upstream fall into three groups. Their SHAs change on every rebase, so
find them by subject (`git log --oneline upstream/master..master`):

| Commit | Keep while… |
|---|---|
| Fixes not yet upstream (e.g. "Render image/binary diffs natively instead of as decoded text") | …upstream hasn't merged an equivalent fix |
| "Make the Build and Release workflow work on forks" | …the fork publishes its own releases |
| "Point updatePlugins.xml at the watbe fork's releases" and CI's "Update plugin repository for release v…" commits | Optional: CI rewrites `updatePlugins.xml` on every fork release anyway |

### Version numbering

Fork releases are named `<upstream version>.<n>`. For example, `v0.8.17.1` is the first fork
release on top of upstream's 0.8.17, and on top of 0.8.18 the next would be `v0.8.18.1`.
IntelliJ ranks that above the upstream release it's based on, and below upstream's next
release, so an upstream release that includes the fork's fixes still reaches the IDE as an update.

**Don't use the workflow's version-bump option** ("Run workflow" with patch/minor/major). It
would publish a version such as `0.8.19`, which clashes with an upstream release, and it
rewrites `CHANGELOG.md` on `master`. Running it with the bump left empty just makes a
snapshot build, which is harmless.

### One-time setup

```bash
git remote add upstream https://github.com/kkkev/jj-idea.git
```

Never run `git push --tags` to `origin`. Upstream's `v*` tags would trigger the fork's
release workflow. CI already fetches upstream's tags into its own clone when it needs them.

### Integrating upstream changes

1. **Fetch and rebase** the fork's commits onto upstream:

   ```bash
   git switch master
   git pull --ff-only origin master   # pick up CI's updatePlugins.xml commits first
   git fetch upstream --tags
   git rebase upstream/master
   ```

   Git skips any fork commit that upstream has since merged identically. If upstream merged
   an *equivalent* but different fix, drop the fork's version when it conflicts
   (`git rebase --skip`).

2. **Resolve conflicts.** During a rebase, `--ours` is upstream (the base being rebased
   onto) and `--theirs` is the fork commit being replayed:
   - **`updatePlugins.xml`**: keep the fork's version (`git checkout --theirs updatePlugins.xml`).
     Upstream's CI regenerates this file on every upstream release, so it conflicts often.
   - **`CHANGELOG.md`**: keep upstream's released sections exactly as they are, and put the
     fork's entries back under `## [Unreleased]`. Never add an entry under a heading upstream
     has already released: `ChangelogReleaseDriftTest` fails on that.
   - **`.github/workflows/build.yml`**: take upstream's changes, then check that every
     fork-specific branch still exists (`grep -n UPSTREAM_REPOSITORY .github/workflows/build.yml`):
     the tag fetch, the preview-key condition, the updatePlugins.xml URLs, and the Marketplace guards.

   Then run `git add <files>` and `git rebase --continue`.

3. **Check the result** locally:

   ```bash
   ./gradlew check
   ./gradlew contractTest   # needs jj on PATH; runs the fork's real-jj tests too
   ```

4. **Push.** A rebase rewrites `master`, so it needs a force-push. `--force-with-lease`
   refuses if CI pushed to `master` since your last fetch:

   ```bash
   git push --force-with-lease origin master
   ```

   Older fork tags stay valid: they keep pointing at their original commits.

5. **Wait for CI to pass on `master`** (`gh run list --repo watbe/jj-idea --branch master`)
   before tagging. The `compat` job tests the supported IDE versions, and `verify` runs
   the plugin verifier.

### Publishing a fork release

```bash
git tag -a v0.8.18.1 -m "v0.8.18.1 (watbe fork)"
git push origin v0.8.18.1
```

Pushing the tag runs the release. If no run starts, trigger one on the tag:
`gh workflow run build.yml --repo watbe/jj-idea --ref v0.8.18.1`. The run then does four things:
- builds the plugin, without the upstream-only preview-code key;
- creates the GitHub Release with the zip;
- uses `## [Unreleased]` from `CHANGELOG.md` as the release notes, because fork tags have no section of their own;
- regenerates `updatePlugins.xml` to point at the release and pushes that to `master`.

It never publishes to the JetBrains Marketplace. Afterwards, run `git pull --ff-only origin master`
so your local `master` has CI's commit.

### When upstream has everything

Once upstream has merged all of the fork's fixes, there's nothing left to publish. Switch
the IDE's plugin repository back to
`https://raw.githubusercontent.com/kkkev/jj-idea/master/updatePlugins.xml`. Upstream's next
release then shows up as an ordinary update, because it ranks above every `x.y.z.n` fork build
of an earlier upstream version.
