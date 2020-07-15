# Dynamic Versioning

Dynamic Versioning, or DynaVer for short, is a specification for versioning which is very intuitive and versatile -- you probably use something very similar to this naturally. DynaVer is a superset of [SemVer](https://github.com/semver/semver), meaning all valid SemVers are valid DynaVers.

## Format
*Note: Values in `<`angle brackets`>` are variable names, values in `[`square brackets`]` are optional, and values in `(`parentheses`)` must select one of the values delimited by a pipe (`|`).*

The following is the general format for a Dynamic Version: `<Number>[<Identifier>][<Metadata>]`. **Number** can be split into between two and four segments: `<Disruptive>.<Incompatible>`, `<Disruptive>.<Incompatible>.<Compatible>`, and `<Disruptive>.<Incompatible>.<Compatible>.<Patch>`.

**Extended layout:**
- `<Disruptive>.<Incompatible>[.<Compatible>[.<Patch>]][(-<Pre>|_<Post>)][(+|~)<Metadata>]`

**Allowed layouts:**
- `<Disruptive>.<Incompatible>` (e.g., `1.0`)
- `<Disruptive>.<Incompatible>-<Pre>` (e.g., `2.3-pre1`)
- `<Disruptive>.<Incompatible>-<Pre>+<Metadata>` (e.g., `0.4-pre1+build5`)
- `<Disruptive>.<Incompatible>_<Post>` (e.g., `1.4_5`)
- `<Disruptive>.<Incompatible>_<Post>+<Metadata>` (e.g., `3.1_2+bump-deps`)
- `<Disruptive>.<Incompatible>+<Metadata>` (e.g., `0.8+5`)
- `<Disruptive>.<Incompatible>.<Compatible>` (e.g., `1.0.8`)
- `<Disruptive>.<Incompatible>.<Compatible>-<Pre>` (e.g., `2.3.0-beta2`)
- `<Disruptive>.<Incompatible>.<Compatible>-<Pre>+<Metadata>` (e.g., `1.6.7-alpha1+2020.05`)
- `<Disruptive>.<Incompatible>.<Compatible>_<Post>` (e.g., `6.1.9_1`)
- `<Disruptive>.<Incompatible>.<Compatible>_<Post>+<Metadata>` (e.g., `2.2.3_1+recompiled`)
- `<Disruptive>.<Incompatible>.<Compatible>+<Metadata>` (e.g., `1.9.0+release.1`)
- `<Disruptive>.<Incompatible>.<Compatible>.<Patch>` (e.g., `2.0.1.3`)
- `<Disruptive>.<Incompatible>.<Compatible>.<Patch>-<Pre>` (e.g., `2.0.3.0-rc3`)
- `<Disruptive>.<Incompatible>.<Compatible>.<Patch>-<Pre>+<Metadata>` (e.g., `0.9.4.2-dev+3`)
- `<Disruptive>.<Incompatible>.<Compatible>.<Patch>_<Post>` (e.g., `1.0.0.1_3`)
- `<Disruptive>.<Incompatible>.<Compatible>.<Patch>_<Post>+<Metadata>` (e.g., `3.1.4.5_1+20200610`)
- `<Disruptive>.<Incompatible>.<Compatible>.<Patch>+<Metadata>` (e.g., `2.0.1.0+1e73fa6`)

### Number

Each part of the **Number** segment must be an integer number (`[0-9]`) and may be zero-padded. Each declared part is delimited by a dot (`.`). The **Number** segment must come first in a version string.

#### Disruptive
The **Disruptive** **Number** must be incremented whenever there is a change made to the project which drastically breaks or disrupts existing functionality for many of your users.

#### Incompatible
The **Incompatible** **Number** must be incremented whenever there is a change made that breaks existing functionality for some users who are following your documentation correctly.

#### Compatible
The optional **Compatible** **Number** must be incremented whenever only backwards-compatable changes are made to the project.

#### Patch
The optional **Patch** **Number** must be incremented whenever only backwards-compatable bug fixes are made which make existing functionality work correctly as described by your documentation.

### Identifier

The **Identifier** segment can either contain **Pre** or **Post**, but not both. The **Identifier** must go after the **Number** segment and before the **Metadata**. **Identifier**s should only contain alphanumeric characters, full stops, underscores and hyphens (`[a-zA-Z0-9._-]`).

#### Pre
The optional **Pre** **Identifier** may be used to mark a version that is currently in development. The **Pre** identifier must be prefixed with a hyphen-minus character (`-`).

#### Post
The optional **Post** **Identifier** may be used to mark a version that only differs in a slight, usually inconsequential way from its parent, such as a hotfix for a version or to fix or tweak the wording of documentation or code comments, but may also be used to mark changes that are for an unknown future release. The **Post** identifier must be prefixed with an underscore (`_`).

### Metadata
The optional **Metadata** segment may be used to mark the metadata of a build. Versions with differing **Metadata** must not have differing features or implementations, and so is recommended for releasing versions for different platforms that need slightly different code or compilation, or for tagging a version with build information or Git hashes. The **Metadata** segment must be prefixed with a plus sign (`+`); if this character is not allowed to be used in your program, or the version is being used in a URL, it may be replaced with a tilde (`~`). **Metadata** should only contain alphanumeric characters, dots, underscores and hyphens (`[a-zA-Z0-9._-]`).

## Incrementing
Each part of a **Number** must increment individually as an integer; the incompatible version after `1.9` is `1.10`, which is not the same as `1.1`. All **Number** parts following the part incremented must be reset (implicitly to `0`); `1.2.1` is followed by `1.3`. Each dot-seperated **Identifier** part should increment in such a way that the new version sorts lexically after the previous version. The **Pre** segment typically follows progressions such as `-alpha` -> `-beta` -> `-rc` or `-dev` -> `-pre`. The **Post** segment is typically a single number representing the recompilation count, such as `_1` -> `_2`, etc.

### Example
`0.0.1` -> `0.0.1.1` -> `0.0.2.0` -> `0.0.1` -> `0.1` -> `0.1.0.1` -> `0.2.0` -> `0.2.1` -> `0.2.1_1` -> ... -> `0.0.9` -> `0.0.10` -> ... -> `1.0-pre1` -> `1.0-pre2` -> ... -> `1.0-pre10` -> `1.0.0-rc` -> `1.0.0+win` & `1.0.0+mac` -> `1.0_1` -> `1.0.0.1` -> `1.0.1.0` -> `1.1-dev+39f2e51` -> `1.1` -> ... -> `1.9.0` -> `1.9.1` -> `1.10` -> ... -> `2.0-rc.1` -> `2.0-rc2` -> `2.0`

## Comparing
Versions should be compared by going across from the right, comparing each **Number** part numerically. When **Compatible** and **Patch** are missing, they default to `0`, such that `1.0-pre2` < `1.0.0-pre3` and `1.6` = `1.6.0` = `1.6.0.0`. Versions with **Pre** **Identifier**s take lower precedence than their corresponding release versions, such that `0.7-pre1` < `0.7`. Versions with **Post** **Indentifier**s take higher precedence than their corresponding release versions, such that `1.6.0` < `1.6_1`. When two versions contain inconsistent dot-seperated **Identifier** parts, the dots should be removed when comparing, such that `1.0-A.B` < `1.0-AC`. When an **Identifier** contains numbers, the numbers should be compared numerically and independently from the surrounding characters, such that `1.4-pre4` < `1.4-pre10`. **Metadata** must be ignored completely when comparing versions.

## Named ranges
Versions in range `0.0.0.*` are Pre-Alpha versions. Versions in this range should probably not be released to the public and may be unstable, or even nonfunctional, with prevalent major bugs. Since there is only one version number to increment, any changes, breaking or patch, can be added in any new version, and so **Pre** and **Post** **Identifier**s may be useful for denoting semantic changes in this range.

Versions in range `0.0.*` are Alpha versions. These versions may be publicly released but may not have been thoroughly tested and may contain major bugs. Versions in this range may increment using the colloquial format `0.0.<Feature>.<Patch>` since "**Compatible**" is deemed redundant without **Incompatible** existing. Versions should now have some semantic meaning with major features not being added in patch releases.

Versions in range `0.*` are Beta versions. These versions are relatively stable and should be able to be safely used but with the expectation of having a few bugs. Versions in this range are incremented using the regular **Number** format but without changes bumping the **Disruptive** part. Versions in this range should now have semantic meaning, following the `0.<Incompatible>.<Compatible>.<Patch>` format.

Versions after `0.*` (`1.*`, `2.*`, etc) are Release versions. These versions are expected to be stable and able to be used without common bugs, and versions have the expectation of having strict semantic meaning, so that a user can declare they want to receive, for example, only `1.6.*` versions, and they should be able to use their software without any incompatible changes breaking it.

A version may be upgraded from one phase to the next at any point, and the new version does not need to match the upgrading criteria applied to release versions. For example, 0.7.3 could be followed by 1.0 which may only contain a single bug fix. This also applies to versions tagged with the **Pre** **Identifier**; for example, 1.1.0-rc3 and 1.1.0 may be the exact same version.
