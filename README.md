# gentoo-syntax-bat

Sublime Text syntax files for [sharkdp/bat](https://github.com/sharkdp/bat), providing
syntax highlighting for Gentoo-specific file formats. Based on the vim syntax definitions
from [gentoo/gentoo-syntax](https://github.com/gentoo/gentoo-syntax).

## Supported File Types

| Syntax | File Types | Description |
|--------|-----------|-------------|
| **Gentoo Ebuild** | `*.ebuild`, `*.eclass` | Ebuild and eclass files (extends Bash) |
| **Gentoo Package Mask** | `package.mask`, `package.unmask` | Package masking files |
| **Gentoo Package Keywords** | `package.keywords`, `package.accept_keywords` | Keyword files |
| **Gentoo Package Use** | `package.use`, `package.env`, `package.license`, `package.properties` | Per-package USE flags and env overrides |
| **Gentoo Make Conf** | `make.conf`, `make.globals` | Portage configuration files |
| **Gentoo Use Desc** | `use.desc`, `use.local.desc` | USE flag description files |
| **Gentoo Mirrors** | `thirdpartymirrors` | Mirror definition files |

> **Note:** `repos.conf` uses standard INI syntax (already supported by bat).
> `metadata.xml` and guide XML use standard XML syntax (already supported by bat).
> `/etc/init.d/`, `/etc/conf.d/`, and `/etc/env.d/` scripts use standard Bash syntax.

## Installation

### 1. Clone or download this repository

```bash
git clone https://github.com/Stinky-c/gentoo-syntax-bat.git
```

### 2. Copy syntax files to bat's syntax directory

```bash
mkdir -p "$(bat --config-dir)/syntaxes"
cp gentoo-syntax-bat/syntaxes/*.sublime-syntax "$(bat --config-dir)/syntaxes/"
```

### 3. Rebuild bat's syntax cache

```bash
bat cache --build
```

### 4. Verify installation

```bash
bat --list-languages | grep -i gentoo
```

You should see output similar to:

```
Gentoo Ebuild (ebuild, eclass)
Gentoo Make Conf (conf, globals)
Gentoo Mirrors
Gentoo Package Keywords (keywords, accept_keywords)
Gentoo Package Mask (mask, unmask)
Gentoo Package Use (use, env, license, properties)
Gentoo Use Desc (desc)
```

## Usage

### Automatic detection

For files with recognised extensions (`.ebuild`, `.eclass`), bat detects the syntax automatically:

```bash
bat my-package.ebuild
bat my-eclass.eclass
```

### Manual language selection

For portage configuration files that use common names without unique extensions, specify the
language explicitly:

```bash
bat --language="Gentoo Package Mask"  /etc/portage/package.mask
bat --language="Gentoo Package Use"   /etc/portage/package.use
bat --language="Gentoo Package Keywords" /etc/portage/package.accept_keywords
bat --language="Gentoo Make Conf"     /etc/portage/make.conf
bat --language="Gentoo Use Desc"      /var/db/repos/gentoo/profiles/use.desc
bat --language="Gentoo Mirrors"       /var/db/repos/gentoo/profiles/thirdpartymirrors
```

### bat configuration file

To make bat automatically use the correct syntax for common portage paths, add mappings to
`$(bat --config-dir)/config`:

```
--map-syntax "*/package.mask:Gentoo Package Mask"
--map-syntax "*/package.unmask:Gentoo Package Mask"
--map-syntax "*/package.accept_keywords:Gentoo Package Keywords"
--map-syntax "*/package.keywords:Gentoo Package Keywords"
--map-syntax "*/package.use:Gentoo Package Use"
--map-syntax "*/package.env:Gentoo Package Use"
--map-syntax "*/package.license:Gentoo Package Use"
--map-syntax "*/package.properties:Gentoo Package Use"
--map-syntax "*/make.conf:Gentoo Make Conf"
--map-syntax "*/make.globals:Gentoo Make Conf"
--map-syntax "*/use.desc:Gentoo Use Desc"
--map-syntax "*/use.local.desc:Gentoo Use Desc"
--map-syntax "*/thirdpartymirrors:Gentoo Mirrors"
```

## Syntax Overview

### Gentoo Ebuild

Extends the built-in Bash syntax and adds highlighting for:

- **EAPI** declaration
- Phase functions: `src_configure`, `src_compile`, `src_install`, `pkg_postinst`, etc.
- Core helpers: `use`, `die`, `econf`, `einfo`, `ewarn`, `eerror`, `elog`, `eapply`, etc.
- Install helpers: `dobin`, `doins`, `doman`, `newbin`, `insinto`, `exeinto`, etc.
- Language variables: `EAPI`, `PV`, `PN`, `P`, `S`, `D`, `FILESDIR`, `DISTDIR`, etc.
- Metadata variables: `SRC_URI`, `DEPEND`, `RDEPEND`, `KEYWORDS`, `IUSE`, etc.
- `inherit` keyword
- Deprecated functions (marked as invalid/error)

### Gentoo Package Files

Package mask/keywords/use files share common highlighting for:

- Comments with embedded email addresses, dates, and bug references
- Package atoms with version operators (`>=`, `<=`, `~`, `=`, etc.)
- Version numbers, SLOT restrictions, and repository qualifiers (`::repo`)

Additional per-format highlighting:
- **Keywords**: stable (`amd64`), testing (`~amd64`), negated (`-amd64`), all (`**`)
- **USE flags**: enabled (`gtk`), disabled (`-gtk`), USE_EXPAND prefixes (`python_targets:`)

### Gentoo Make Conf

Shell-like assignment syntax with highlighting for:

- Known portage variables (`USE`, `CFLAGS`, `ACCEPT_KEYWORDS`, `FEATURES`, etc.)
- Discouraged variables (`LDFLAGS`, `ARCH`, etc.)
- Quoted string values with variable interpolation
- USE flag lists within `USE="..."` assignments

## License

This project's syntax files are provided under the same terms as
[gentoo/gentoo-syntax](https://github.com/gentoo/gentoo-syntax) (Vim license / free redistribution).
