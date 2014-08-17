## urimark v0.3.0.0 [GNU GPLv3]

TODO

### Index

1. [Install](#install)
2. [Usage](#usage)
3. [Examples](#examples)
4. [Storage](#storage)
5. [Enviroment](#enviroment)
6. [Configurations](#configurations)
  * [Hooks](#hooks)
7. [Notes](#notes)
8. [TODO](#todo)
9. [Bugs & Requests](#bugs--requests)

### Install

`urimark`(1) was written on `Debian Testing` with `GNU bash`, `GNU grep`, `GNU sed`, `GNU coreutils`, `GNU diffutils` and `GNU findutils`. Explicitly required: `comm`, `cp`, `cut`, `date`, `diff`, `GNU bash >= 4.0`, `GNU find`, `GNU sed`, `GNU xargs`, `grep`, `head`, `md5sum`, `mkdir`, `od`, `rm`, `sort`, `tail`, `tr`, `uniq`.

* Get `urimark`(1) with `$ git clone https://github.com/D630/urimark.git` or
  download it on https://github.com/D630/urimark/releases
* Copy the script `um` elsewhere into `<PATH>` and make it executable.

### Usage

```
um (-a|-d|-e|-h|-m|-r|-v) [<OPT>...]
um [<OPT>...] [<HOOK>]

SUBCOMMANDS
-----------
    ACTION                      OPT
    ------                      ---
    -a, --add                   -4,-7,-D,-H,-N,-U
    -d, --delete                -0,-1,-2,-3,-7,-A,-D,-H,-M,-N,-n,-!,-0,
                                -P,-S,-U,-Y
    -e, --edit                  -0,-1,-2,-3,-7,-A,-D,-H,-M,-N,-!,-0,-P,
                                -S,-U,-Y
    -h, --help
    -m, --modify                -0,-1,-2,-3,-4,-5,-6,-7,-8,-9,-A,-D,-H,
                                -M,-N,-n,-!,-0,-P,-S,-U,-Y
    -r, --rebuild               -0,-1,-2,-3,-7,-A,-D,-H,-M,-N,-n,-!,-0,
                                -P,-S,-U,-x,-Y
    -v, --version

    OPT                         ARG
    ---                         ---
    -0, --uuid=                 <UUID>
    -1, --id=                   <ID>
    -2, --bd=                   <BD>
    -3, --md=                   <MD>
    -4, --tag=                  <TAG>
    -5, --tag+=                 <TAG>
    -6, --tag-=                 <TAG>
    -7, --reference=            <REF>
    -8, --reference+=           <REF>
    -9, --reference-=           <REF>
    -A, --and
    -D, --description=          <DESC>
    -H, --hierarchy=            <HIER>
    -M, --mod
    -N, --name=                 <NAME>
    -n, --non-interactive
    -!, --not
    -O, --or
    -P, --part=                 <PART>
    -S, --scheme=               <SCHEME>
    -U, --uri=                  <URI>
    -x, --index
    -Y, --authority=            <AUTH>

ARGUMENTS
---------
    <AUTH>*                     'string'
    <BD>                        'date'* or range 'date;date'. Two special
                                values: 'first' and 'last'
    <DESC>*                     'string'
    <HIER>*                     '/foo/bar/n'
    <HOOK>                      'string' without space character
    <ID>*                       'int', 'int,int', range 'int-int'
                                (or combi). Two special values: 'first'
                                and 'last'
    <MD>                        'date'* or range 'date;date'. Two special
                                values: 'first' and 'last'
    <NAME>*                     'string'
    <PART>*                     'string'
    <REF>*                      'int' or 'int;int;n'
    <SCHEME>*                   'http','https','ftp','ftps','dav','davs',
                                'gopher','webdav','webdavs
    <TAG>*                      'string' or 'string;string;n'
    <URI>                       'string'
    <UUID>*                     'uuid'

    *regextype: posix-egrep (not used with -a)
```

### Examples

TODO

### Storage

Currently, `urimark`(1) can handle URLs with these protocols: http, https, ftp, ftps, dav, davs, gopher, webdav, webdavs. To store a record, an URL will be split up into the three separate fields `scheme`, `authority` and `part`. All records with the same `authority` share the same `uuid`; but every URL has its own line counted `id`. A complete record is constructed like in this example database with three data sets:

```
# $ um info

UUID       42712582202233536417
ID         1
BD         1408100780 # build date
MD         1408282460 # last modification date
SCHEME     https
AUTHORITY  bitbucket.org
PART       /
NAME       Plant your code in the cloud. Watch it grow.
DESC       web-based hosting service for projects that use either the Mercurial or Git revision control systems.
HIER       /software/internet/web/crc
TAGS       hg;mercurial;git;web;code;collaboration;vcs;atlassian
REF        2

UUID       42720785481233211538
ID         2
BD         1408102469
MD         1408102469
SCHEME     https
AUTHORITY  github.com
PART       /
NAME       GitHub. Build software better, together.
DESC       Git repository web-based hosting service which offers revision control and source code management functionality of Git.
HIER       /software/internet/web/crc
TAGS       git;web;code;collaboration;vcs
REF        3;1

UUID       42720785481233211538
ID         3
BD         1408102469
MD         1408102469
SCHEME     https
AUTHORITY  github.com
PART       /blog
NAME       The GitHub Blog
DESC       Tech and Info blog
HIER       /media/blogs/computer
TAGS       github;git;blog
REF        2
```

The storage backend is a combination of comma-separated values (`CSV`) and `bash` parameters, hierarchically arranged in your file system:

```
# $ tree -a -P "*" -n --noreport -L 20 --charset=ascii "$PWD"

${URIMARK_DATA_DIR}
|-- 42712582202233536417
|   |-- 1
|   `-- _index
|-- 42720785481233211538
|   |-- 2
|   |-- 3
|   `-- _index
`-- _index
```

```bash
# $ cat "${URIMARK_DATA_DIR}/42720785481233211538/2"

uuid=42720785481233211538
date_build=1408102469
date_modification=1408102469
uri_authority="github.com"
uri_scheme="https"
uri_scheme_specific_part="/"
uri_scheme_specific_part_description="Git repository web-based hosting service which offers revision control and source code management functionality of Git."
uri_scheme_specific_part_hierarchy="/software/internet/web/crc"
uri_scheme_specific_part_name="GitHub. Build software better, together."
uri_scheme_specific_part_id=2
uri_scheme_specific_part_reference[0]=3
uri_scheme_specific_part_reference[1]=1
uri_scheme_specific_part_tag[0]="git"
uri_scheme_specific_part_tag[1]="web"
uri_scheme_specific_part_tag[2]="code"
uri_scheme_specific_part_tag[3]="collaboration"
uri_scheme_specific_part_tag[4]="vcs"
```

```
# $ cat "${URIMARK_DATA_DIR}/42720785481233211538/_index"

"42720785481233211538"|"2"|"1408102469"|"1408102469"|"https"|"github.com"|"/"|"GitHub. Build software better, together."|"Git repository web-based hosting service which offers revision control and source code management functionality of Git."|"/software/internet/web/crc"|"git;web;code;collaboration;vcs"|"3;1"
"42720785481233211538"|"3"|"1408102469"|"1408102469"|"https"|"github.com"|"/blog"|"The GitHub Blog"|"Tech and Info blog"|"/media/blogs/computer"|"github;git;blog"|"2"
```

```
# $ cat "${URIMARK_DATA_DIR}/_index"

"42720785481233211538"|"2"|"1408102469"|"1408102469"|"https"|"github.com"|"/"|"GitHub. Build software better, together."|"Git repository web-based hosting service which offers revision control and source code management functionality of Git."|"/software/internet/web/crc"|"git;web;code;collaboration;vcs"|"3;1"
"42720785481233211538"|"3"|"1408102469"|"1408102469"|"https"|"github.com"|"/blog"|"The GitHub Blog"|"Tech and Info blog"|"/media/blogs/computer"|"github;git;blog"|"2"
"42712582202233536417"|"1"|"1408100780"|"1408282460"|"https"|"bitbucket.org"|"/"|"Plant your code in the cloud. Watch it grow."|"web-based hosting service for projects that use either the Mercurial or Git revision control systems."|"/software/internet/web/crc"|"hg;mercurial;git;web;code;collaboration;vcs;atlassian"|"2"
```

### Enviroment

| **evar**  | **default val** |
| --------- | --------------- |
| `URIMARK_CONF_FILE` | `${XDG_CONFIG_HOME:-"${HOME}/.config"}/urimark/urimark.conf` |
| `URIMARK_DATA_DIR` | `${XDG_DATA_HOME:-"${HOME}/.local/share"}/urimark/data` |
| `URIMARK_TMP_DIR` | `${TMPDIR:-"/tmp"}/urimark` |

### Configurations

Along with this programme comes an exemplary [configuration file](../master/doc/examples/urimark.conf). The configurations will be sourced after command line parsing. You can set following parameters:

* normal scalar variables
    * `description_default=`: used with the subcommand `add`. Fallback: `null`
    * `editor=`: used with the subcommand `edit`. Fallback: `<EDITOR>`
    * `hierarchy_default=`: used with the subcommand `add`. Fallback: `null`
    * `hook_default=`: used, when no subcommand has been specified. Fallback: `um_default`
    * `name_default=`: used with the subcommand `add`. Fallback: `null`
    * `reference_default=`: used with the subcommand `add`. Fallback: `null`
    * `tag_default=`: used with the subcommand `add`. Fallback: `null`
* associative array variables
    * `hook[<NAME> description]=<STRING>`
    * `hook[<NAME> header]=<COMMAND>`
    * `hook[<NAME> preprocess]=<COMMAND>`
    * `hook[<NAME> filter]=<FILTER>`
    * `hook[<NAME> postprocess]=<COMMAND>`
    * `hook[<NAME> footer]=<COMMAND>`

#### Hooks

A hook is a set of connected subscripts of an associative array called `hook`; a valid hook needs to have a name (string without space character) and a description. Hooks come into play, when there is no regular subcommand on command line. If a hook has no specified filter, the filter on the command line will be used (`um [<FILTER>] <HOOK>`; `<FILTER>`: `-0,-1,-2,-3,-7,-A,-D,-H,-M,-N,-!,-O,-P,-S,-U,-Y`). Hooks will be called in the function `__um_query_post()`:

```bash
[[ ${hook[${hook_choosen} header]} ]] && eval "${hook[${hook_choosen} header]}"
eval "${hook[${hook_choosen} preprocess]} __um_query_post_result ${hook[${hook_choosen} postprocess]}"
[[ ${hook[${hook_choosen} footer]} ]] && eval "${hook[${hook_choosen} footer]}"
```

Since hooks work with `eval`, you need to care about the right quoting. It is also possible to declare functions in the configuration file and to use them as command in the value of the arrays.

If there is no default hook in the configuration file, the builtin report `um_default` will be used instead:

```bash
hook[um_default description]="Builtin default report"
hook[um_default postprocess]="| cut -d '|' -f1,2 | tr -d '\"' | tr '|' ' '"
```

The exemplary [configuration file](../master/doc/examples/urimark.conf) declares following hooks:

| **Name**  | **Description** |
| --------- | --------------- |
| `abso` | `Report: Absolute Path` |
| `acloud` | `Report: Cloud of authorities` |
| `browse` | `Interaction: Browse URI with urlview` |
| `fmod` | `Report: First 10 modified records with ID,MD,URI` |
| `hcloud` | `Report: Cloud of hierarchies` |
| `ids` | `Report: Show first and last id` |
| `__info` | `Report: All with line break` |
| `_info` | `Report: All with line break, labels and empty lines` |
| `info` | `Report: All with line break, labels, empty lines and formatted with column` |
| `itags` | `Report: ID Tags` |
| `iuri` | `Report: ID URI` |
| `__list` | `Report: All as list and space as delimiter` |
| `_list` | `Report: All as list with labels and space as delimiter` |
| `list` | `Report: All with labels and formatted with column` |
| `lmod` | `Report: Last 10 modified records with ID,MD,URI` |
| `_meta` | `Report: ID, Namen, Desc, Hierarchy, Tags and References and space as delimiter` |
| `newest` | `Report: Last 10 newest records: ID,BD,URI` |
| `number` | `Report: Number of Authorities and IDs` |
| `oldest` | `Report: Last 10 oldest records: ID,BD,URI` |
| `pcloud` | `Report: Cloud of parts` |
| `rcloud` | `Report: Cloud of references` |
| `scloud` | `Report: Cloud of schemes` |
| `tcloud` | `Report: Cloud of tags` |
| `_tlist` | `Report: ID, MD, BD and formatted with column` |
| `uri` | `Report: URI` |

### Notes

TODO

### TODO

See file [TODO](../master/doc/TODO), which comes along with this programme.

### Bugs & Requests

Report it on https://github.com/D630/urimark
