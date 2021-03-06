## v5.0.1 (2017-05-31):

Hey y'all! Hope you're enjoying the new npm!

As you all know, fresh software that's gone through major overhauls tends to
miss a lot of spots the old one used to handle well enough, and `npm@5` is no
exception. The CLI team will be doing faster release cycles that go directly to
the `latest` tag for a couple of weeks while 5 stabilizes a bit and we're
confident the common low-hanging fruit people are running into are all taken
care of.

With that said: this is our first patch release! The biggest focus is fixing up
a number of git-related issues that folks ran into right out the door. It also
fixes other things, like some proxy/auth-related issues, and even has a neat
speed boost! (You can expect more speed bumps in the coming releases as pending
work starts landing, too!)

Thanks everyone who's been reporting issues and submitting patches!

### BUGFIXES

* [`e61e68dac`](https://github.com/npm/npm/commit/e61e68dac4fa51c0540a064204a75b19f8052e58)
  [#16762](https://github.com/npm/npm/pull/16762)
  Make `npm publish` obey the `--tag` flag again.
  ([@zkat](https://github.com/zkat))
* [`923fd58d3`](https://github.com/npm/npm/commit/923fd58d312f40f8c17b232ad1dfc8e2ff622dbd)
  [#16749](https://github.com/npm/npm/pull/16749)
  Speed up installations by nearly 20% by... removing one line of code. (hah)
  ([@mikesherov](https://github.com/mikesherov))
* [`9aac984cb`](https://github.com/npm/npm/commit/9aac984cbbfef22182ee42b51a193c0b47146ad6)
  Guard against a particular failure mode for a bug still being hunted down.
  ([@iarna](https://github.com/iarna))
* [`80ab521f1`](https://github.com/npm/npm/commit/80ab521f18d34df109de0c5dc9eb1cde5ff6d7e8)
  Pull in dependency updates for various core deps:
  * New `pacote` fixes several git-related bugs.
  * `ssri` update fixes crash on early node@4 versions.
  * `make-fetch-happen` update fixes proxy authentication issue.
  * `npm-user-validate` adds regex for blocking usernames with illegal chars.
  ([@zkat](https://github.com/zkat))
* [`7e5ce87b8`](https://github.com/npm/npm/commit/7e5ce87b84880c7433ee4c07d2dd6ce8806df436)
  `pacote@2.7.26`:
  Fixes various other git issues related to commit hashes.
  ([@zkat](https://github.com/zkat))
* [`acbe85bfc`](https://github.com/npm/npm/commit/acbe85bfc1a68d19ca339a3fb71da0cffbf58926)
  [#16791](https://github.com/npm/npm/pull/16791)
  `npm view` was calling `cb` prematurely and giving partial output when called
  in a child process.
  ([@zkat](https://github.com/zkat))
* [`ebafe48af`](https://github.com/npm/npm/commit/ebafe48af91f702ccefc8c619d52fed3b8dfd3c7)
  [#16750](https://github.com/npm/npm/pull/16750)
  Hamilpatch the Musical: Talk less, complete more.
  ([@aredridel](https://github.com/aredridel))

### DOCUMENTATION

* [`dc2823a6c`](https://github.com/npm/npm/commit/dc2823a6c5fc098041e61515c643570819d059d2)
  [#16799](https://github.com/npm/npm/pull/16799)
  Document that `package-lock.json` is never allowed in tarballs.
  ([@sonicdoe](https://github.com/sonicdoe))
* [`f3cb84b44`](https://github.com/npm/npm/commit/f3cb84b446c51d628ee0033cdf13752c15b31a29)
  [#16771](https://github.com/npm/npm/pull/16771)
  Fix `npm -l` usage information for the `test` command.
  ([@grawlinson](https://github.com/grawlinson))

### OTHER CHANGES

* [`661262309`](https://github.com/npm/npm/commit/66126230912ab5ab35287b40a9908e036fa73994)
  [#16756](https://github.com/npm/npm/pull/16756)
  remove unused argument
  ([@Aladdin-ADD](https://github.com/Aladdin-ADD))
* [`c3e0b4287`](https://github.com/npm/npm/commit/c3e0b4287ea69735cc367aa7bb7e7aa9a6d9804b)
  [#16296](https://github.com/npm/npm/pull/16296)
  preserve same name convention for command
  ([@desfero](https://github.com/desfero))
* [`9f814831d`](https://github.com/npm/npm/commit/9f814831d330dde7702973186aea06caaa77ff31)
  [#16757](https://github.com/npm/npm/pull/16757)
  remove unused argument
  ([@Aladdin-ADD](https://github.com/Aladdin-ADD))
* [`3cb843239`](https://github.com/npm/npm/commit/3cb8432397b3666d88c31131dbb4599016a983ff)
  minor linter fix
  ([@zkat](https://github.com/zkat))

## v5.0.0 (2017-05-25)

Wowowowowow npm@5!

This release marks months of hard work for the young, scrappy, and hungry CLI
team, and includes some changes we've been hoping to do for literally years.
npm@5 takes npm a pretty big step forward, significantly improving its
performance in almost all common situations, fixing a bunch of old errors due to
the architecture, and just generally making it more robust and fault-tolerant.
It comes with changes to make life easier for people doing monorepos, for users
who want consistency/security guarantees, and brings semver support to git
dependencies. See below for all the deets!

### Breaking Changes

* Existing npm caches will no longer be used: you will have to redownload any cached packages. There is no tool or intention to reuse old caches. ([#15666](https://github.com/npm/npm/pull/15666))

* `npm install ./packages/subdir` will now create a symlink instead of a regular installation. `file://path/to/tarball.tgz` will not change -- only directories are symlinked. ([#15900](https://github.com/npm/npm/pull/15900))

* npm will now scold you if you capitalize its name. seriously it will fight you.

* [npm will `--save` by default now](https://twitter.com/maybekatz/status/859229741676625920). Additionally, `package-lock.json` will be automatically created unless an `npm-shrinkwrap.json` exists. ([#15666](https://github.com/npm/npm/pull/15666))

* Git dependencies support semver through `user/repo#semver:^1.2.3` ([#15308](https://github.com/npm/npm/pull/15308)) ([#15666](https://github.com/npm/npm/pull/15666)) ([@sankethkatta](https://github.com/sankethkatta))

* Git dependencies with `prepare` scripts will have their `devDependencies` installed, and `npm install` run in their directory before being packed.

* `npm cache` commands have been rewritten and don't really work anything like they did before. ([#15666](https://github.com/npm/npm/pull/15666))

* `--cache-min` and `--cache-max` have been deprecated. ([#15666](https://github.com/npm/npm/pull/15666))

* Running npm while offline will no longer insist on retrying network requests. npm will now immediately fall back to cache if possible, or fail. ([#15666](https://github.com/npm/npm/pull/15666))

* package locks no longer exclude `optionalDependencies` that failed to build. This means package-lock.json and npm-shrinkwrap.json should now be cross-platform. ([#15900](https://github.com/npm/npm/pull/15900))

* If you generated your package lock against registry A, and you switch to registry B, npm will now try to [install the packages from registry B, instead of A](https://twitter.com/maybekatz/status/862834964932435969). If you want to use different registries for different packages, use scope-specific registries (`npm config set @myscope:registry=https://myownregist.ry/packages/`). Different registries for different unscoped packages are not supported anymore.

* Shrinkwrap and package-lock no longer warn and exit without saving the lockfile.

* Local tarballs can now only be installed if they have a file extensions `.tar`, `.tar.gz`, or `.tgz`.

* A new loglevel, `notice`, has been added and set as default.

* One binary to rule them all: `./cli.js` has been removed in favor of `./bin/npm-cli.js`. In case you were doing something with `./cli.js` itself. ([#12096](https://github.com/npm/npm/pull/12096)) ([@watilde](https://github.com/watilde))

* Stub file removed ([#16204](https://github.com/npm/npm/pull/16204)) ([@watilde](https://github.com/watilde))

* The "extremely legacy" `_token` couchToken has been removed. ([#12986](https://github.com/npm/npm/pull/12986))

### Feature Summary

#### Installer changes

* A new, standardised lockfile feature meant for cross-package-manager compatibility (`package-lock.json`), and a new format and semantics for shrinkwrap. ([#16441](https://github.com/npm/npm/pull/16441))

* `--save` is no longer necessary. All installs will be saved by default. You can prevent saving with `--no-save`. Installing optional and dev deps is unchanged: use `-D/--save-dev` and `-O/--save-optional` if you want them saved into those fields instead. Note that since npm@3, npm will automatically update npm-shrinkwrap.json when you save: this will also be true for `package-lock.json`. ([#15666](https://github.com/npm/npm/pull/15666))

* Installing a package directory now ends up creating a symlink and does the Right Thing™ as far as saving to and installing from the package lock goes. If you have a monorepo, this might make things much easier to work with, and probably a lot faster too. 😁 ([#15900](https://github.com/npm/npm/pull/15900))

* Project-level (toplevel) `preinstall` scripts now run before anything else, and can modify `node_modules` before the CLI reads it.

* Two new scripts have been added, `prepack` and `postpack`, which will run on both `npm pack` and `npm publish`, but NOT on `npm install` (without arguments). Combined with the fact that `prepublishOnly` is run before the tarball is generated, this should round out the general story as far as putzing around with your code before publication.

* Git dependencies with `prepare` scripts will now [have their devDependencies installed, and their prepare script executed](https://twitter.com/maybekatz/status/860363896443371520) as if under `npm pack`.

* Git dependencies now support semver-based matching: `npm install git://github.com/npm/npm#semver:^5` (#15308, #15666)

* `node-gyp` now supports `node-gyp.cmd` on Windows ([#14568](https://github.com/npm/npm/pull/14568))

* npm no longer blasts your screen with the whole installed tree. Instead, you'll see a summary report of the install that is much kinder on your shell real-estate. Specially for large projects. ([#15914](https://github.com/npm/npm/pull/15914)):
```
$ npm install
npm added 125, removed 32, updated 148 and moved 5 packages in 5.032s.
$
```

* `--parseable` and `--json` now work more consistently across various commands, particularly `install` and `ls`.

* Indentation is now [detected and preserved](https://twitter.com/maybekatz/status/860690502932340737) for `package.json`, `package-lock.json`, and `npm-shrinkwrap.json`. If the package lock is missing, it will default to `package.json`'s current indentation.

#### Publishing

* New [publishes will now include *both* `sha512`](https://twitter.com/maybekatz/status/863201943082065920) and `sha1` checksums. Versions of npm from 5 onwards will use the strongest algorithm available to verify downloads. [npm/npm-registry-client#157](https://github.com/npm/npm-registry-client/pull/157)

#### Cache Rewrite!

We've been talking about rewriting the cache for a loooong time. So here it is.
Lots of exciting stuff ahead. The rewrite will also enable some exciting future
features, but we'll talk about those when they're actually in the works. #15666
is the main PR for all these changes. Additional PRs/commits are linked inline.

* Package metadata, package download, and caching infrastructure replaced.

* It's a bit faster. [Hopefully it will be noticeable](https://twitter.com/maybekatz/status/865393382260056064). 🤔

* With the shrinkwrap and package-lock changes, tarballs will be looked up in the cache by content address (and verified with it).

* Corrupted cache entries will [automatically be removed and re-fetched](https://twitter.com/maybekatz/status/854933138182557696) on integrity check failure.

* npm CLI now supports tarball hashes with any hash function supported by Node.js. That is, it will [use `sha512` for tarballs from registries that send a `sha512` checksum as the tarball hash](https://twitter.com/maybekatz/status/858137093624573953). Publishing with `sha512` is added by [npm/npm-registry-client#157](https://github.com/npm/npm-registry-client/pull/157) and may be backfilled by the registry for older entries.

* Remote tarball requests are now cached. This means that even if you're missing the `integrity` field in your shrinkwrap or package-lock, npm will be able to install from the cache.

* Downloads for large packages are streamed in and out of disk. npm is now able to install packages of """any""" size without running out of memory. Support for publishing them is pending (due to registry limitations).

* [Automatic fallback-to-offline mode](https://twitter.com/maybekatz/status/854176565587984384). npm will seamlessly use your cache if you are offline, or if you lose access to a particular registry (for example, if you can no longer access a private npm repo, or if your git host is unavailable).

* A new `--prefer-offline` option will make npm skip any conditional requests (304 checks) for stale cache data, and *only* hit the network if something is missing from the cache.

* A new `--prefer-online` option that will force npm to revalidate cached data (with 304 checks), ignoring any staleness checks, and refreshing the cache with revalidated, fresh data.

* A new `--offline` option will force npm to use the cache or exit. It will error with an `ENOTCACHED` code if anything it tries to install isn't already in the cache.

* A new `npm cache verify` command that will garbage collect your cache, reducing disk usage for things you don't need (-handwave-), and will do full integrity verification on both the index and the content. This is also hooked into `npm doctor` as part of its larger suite of checking tools.

* The new cache is *very* fault tolerant and supports concurrent access.
  * Multiple npm processes will not corrupt a shared cache.
  * Corrupted data will not be installed. Data is checked on both insertion and extraction, and treated as if it were missing if found to be corrupted. I will literally bake you a cookie if you manage to corrupt the cache in such a way that you end up with the wrong data in your installation (installer bugs notwithstanding).
  * `npm cache clear` is no longer useful for anything except clearing up disk space.

* Package metadata is cached separately per registry and package type: you can't have package name conflicts between locally-installed packages, private repo packages, and public repo packages. Identical tarball data will still be shared/deduplicated as long as their hashes match.

* HTTP cache-related headers and features are "fully" (lol) supported for both metadata and tarball requests -- if you have your own registry, you can define your own cache settings the CLI will obey!

* `prepublishOnly` now runs *before* the tarball to publish is created, after `prepare` has run.
