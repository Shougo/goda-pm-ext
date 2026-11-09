# goda-pm-ext

The protocols and the exts of
[goda-pm.vim](https://github.com/Shougo/goda-pm.vim): Go packages for its
configuration (they have no Vim script: they are not Vim plugins).

```go
import (
	"github.com/Shougo/goda-pm-ext/ext/installer"
	"github.com/Shougo/goda-pm-ext/ext/local"
	"github.com/Shougo/goda-pm-ext/ext/protocol/git"
	"github.com/Shougo/goda-pm-ext/ext/protocol/http"
	"github.com/Shougo/goda-pm-ext/ext/toml"
)
```

The lazy loading is
[goda-pm-ext-lazy.vim](https://github.com/Shougo/goda-pm-ext-lazy.vim): it
has Vim script.

## Protocols

### git

`ext/protocol/git`: git repositories, as dpp-protocol-git. `user/name` is a
repository of the hub site (GitHub); an absolute path to a repository is a
local plugin. The plugins go in `{base-path}/repos/{host}/{user}/{name}`.

- Params (`protocolParams.git`): `cloneDepth`, `commandPath`,
  `defaultHubSite`, `defaultProtocol`, `defaultRemote`,
  `enableCredentialHelper`, `enablePartialClone`, `enableSSLVerify`,
  `pullArgs`.
- Plugin attrs (`protocolAttrs`): `gitDefaultBranch`, `gitRemote`.

### http

`ext/protocol/http`: a file or an archive (zip, tar.gz, tar.bz2) on the web,
as dpp-protocol-http: `rev` is the SHA-1 of what is downloaded. goda-pm
downloads and extracts it itself (no curl, unzip nor tar is needed).

## Exts

### installer

`ext/installer`: it installs, updates and builds the plugins, as
dpp-ext-installer.

- Actions: `build`, `checkNotUpdated`, `denoCache`, `getFailed`, `getLogs`,
  `getNotInstalled`, `getNotUpdated`, `getUpdateLogs`, `getUpdated`,
  `install`, `reinstall`, `update`. The params of `install`, `reinstall` and
  `update`: `names` (the plugins, all by default) and `rollback` (the date of
  a rollback file, or `latest`); of `checkNotUpdated`: `names` and `force`
  (update without asking).
- Params (`extParams.installer`): `checkDiff`, `checkExts`, `logFilePath`,
  `maxInactiveDays`, `maxProcesses`, `minCommitDays`, `wait`.
- Plugin attrs (`extAttrs`): `installerBuild`, `installerFrozen`,
  `installerMaxInactiveDays`, `installerMinCommitDays`.

```vim
call goda_pm#async_ext_action('installer', 'install')
call goda_pm#async_ext_action('installer', 'update')
```

These are not exts with actions, but Go functions of the configuration:

### toml

`ext/toml`: `toml.Load()` loads a TOML file of dpp-ext-toml (the same files
work). Highlight their hooks with `goda_pm#toml_syntax()` of goda-pm.vim.

### local

`ext/local`: `local.Load()` gives the plugins of a directory (those you
develop), as dpp-ext-local.

## goda-fi source

`ext/fi/pmfi` is the source `goda_pm` of
[goda-fi.vim](https://github.com/Shougo/goda-fi.vim), as ddu-source-dpp: the
plugins, as items of the kind `file`, with the actions `browse` and `update`.
Add it in your configuration of goda-fi:

```go
b.AddSource(pmfi.New())
```
