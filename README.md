# conventions

> Create repositories with pluggable `conventions`
> WIP

* [Overview](#overview)
  * [A `convention`](#a-convention)
  + [Example A: Base `convention`](#the--base-convention-)
  + [Example B: Semver `convention`](#another--convention-)
  + [Example C: Conventional Commits `convention`](#another--convention-)
  + [Example D: unit-testing `convention`](#another--convention-)
  + [Example E: Whatever `convention`](#another--convention-)
* [Usage](#usage)
* [Flow](#flow)
* [Notes](#notes)
* [Todo](#todo)


## Overview

A CLI app that uses the [Github API][gapi] to create repositories.

The repositories are described as a list of pluggable, 
user-definable `conventions`.

> example: A `base` plus 4 different `conventions`:

```
├─ conventions/
   ├─ base/
   ├─ unit-testing/
   ├─ conventional-commits/
   ├─ semver/
   ├─ github-flow/
```

## A `convention`

A `convention` is a self-contained folder describing a general 
[convention][convention],   
for example: [*Conventional Commits*][ccomits].

It's a *partial* repository structure with all the necessary  
documents and files to support the convention:

Each `convention` self-contains all the necessary:

- Documents
  - [README.md][readme]
  - [guides][guides]
  - [policies][secpolic]
  - etc
- [rulesets][rulesets]
- [workflows][actions]
- configurations 
- etc. 

to add that convention to the repository.

### The `base convention` 

The `base convention` builds the minimally-working "skeleton" repository, 
hence it's always required:

```
base
├── .github
│   ├── CONTRIBUTING.md
│   └── SECURITY.md
├── rulesets
│   └── protected-branch.json
├── src
│   └── index.js
├── README.md
└── package.json
```

### Example B: [Semver `convention`][semver]

- Adds a [workflow][actions] to publish to `npm` automatically
- Adds a [CONTRIBUTING.md][guides] section to explain this convention
- Adds a [ruleset][rulesets] to restrict tag versioning to semver formatting

```
semver
├── .github
│   ├── CONTRIBUTING.md
│   └── workflows
│       └── npm-publish.yml
└── rulesets
    └── semantic-tags.json
```

### Example C: [Conventional Commits `convention`][ccomits]

- Adds a [CONTRIBUTING.md][guides] section to explain this convention
- Adds a [ruleset][rulesets] to restrict commit messages to follow the 
  prescribed format

```
conventional-commmits
├── .github
│   └── CONTRIBUTING.md
└── rulesets
    └── conventional-commits.json
```

### Example D: [unit-testing `convention`][ccomits]

- Adds a `test` folder and a `basic.test.js` unit-test.
- Adds a `package.json` that adds a line:
```json
"scripts": {
  "test": "node --test"
}
```
- Adds a [workflow][actions] to run the unit tests on CI.
- Adds a [CONTRIBUTING.md][guides] section to explain this convention
- Adds a [ruleset][rulesets] to restrict merge of PRs only if unit-tests
  pass.

```
unit-testing
├── .github
│   ├── workflows
│   │   └── test.yml
│   └── CONTRIBUTING.md    
├── test
│   └── basic.test.js
├── rulesets
│   └── pr-status-checks.json
├── README.md
└── package.json
```

### Example E: [whatever `convention`][ccomits]

Conventions are user-definable so anything goes.

## Usage

Run this CLI app and point to a `conventions` folder:

```bash
node --run new --conventions=./conventions
```

which should produce this repository:

```
repository
├── .github
│   ├── workflows
│   │   ├── npm-publish.yml
│   │   └── test.yml
│   ├── CONTRIBUTING.md    
│   └── SECURITY.md    
├── test
│   └── basic.test.js
├── rulesets
│   ├── protected-branch.json
│   ├── conventional-commits.json
│   ├── pr-status-checks.json
│   └── semantic-tags.json
├── README.md
└── package.json
```

This repository now supports:

- [Semver][semver]:
  - a workflow to `publish.yml` to `npm` 
  - rulesets so tags can only use semantic versioning tag numbers
  - sections in `CONTRIBUTING` detailing this convention

- [Conventional Commits][ccomits]:
  - rulesets so commits can only use conventional commit message formats
  - sections in `CONTRIBUTING` detailing this convention
  
- [Unit tests][testing]:
  - Basic unit-tests runnable via `node --run test`
  - unit-tests that run on CI via a `test.yml` workflow
  - rulesets that require unit-tests passing before merge.
  - sections in `CONTRIBUTING` & `README` detailing this convention

## Flow

- Markdown documents (`README.md`, `CONTRIBUTING.md` etc...) are merged
- Rulesets are uploaded as-is
- Workflows are added as-is in the `.gitub/workflows/<workflow>.yml`
- `package.json` is merged
- All other documents are added as-is.
  
## Notes

- Github API extensions are in: `./extensions`
- The base convention is `base`
- Conventions can be ordered by prefixing their filename with a number:
  - i.e: `1-conventional-commits`, `2-github-flow`, etc...

## Todo

- [ ] Add tests
- [ ] Merge `Document`
- [ ] Merge `JSON`
- [ ] Use the `--conventions` params
- [ ] Fix rulesets (waiting for Github support reply)

## Authors

[@nicholaswmin][author-url]

## License 

The [MIT License][license]

[convention]: https://en.wikipedia.org/wiki/Coding_conventions#
[ccomits]: https://www.conventionalcommits.org/en/v1.0.0/
[semver]: https://semver.org/
[gapi]: https://docs.github.com/en/rest?apiVersion=2022-11-28

[rulesets]: https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets
[actions]: https://docs.github.com/en/actions/writing-workflows
[secpolic]: https://docs.github.com/en/code-security/getting-started/adding-a-security-policy-to-your-repository
[readme]: https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes
[guides]: https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/setting-guidelines-for-repository-contributors
[testing]: https://en.wikipedia.org/wiki/Unit_testing

[author-url]: https://github.com/nicholaswmin
[license]: ./LICENSE
