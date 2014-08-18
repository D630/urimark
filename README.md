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

`urimark`(1) was written on `Debian Testing` with `GNU bash`, `GNU grep`, `GNU sed`, `GNU coreutils`, `GNU diffutils` and `GNU findutils`. Explicitly required: `comm`, `cp`, `cut`, `date`, `diff`, `GNU bash >= 4.0`, `GNU find`, `GNU sed`, `GNU xargs`, `grep`, `head`, `md5sum`, `mkdir`, `od`, `rm`, `sort`, `stat`, tail`, `tr`, `uniq`.

* Get `urimark`(1) with `$ git clone https://github.com/D630/urimark.git` or
  download it on https://github.com/D630/urimark/releases
* Copy the script `um` elsewhere into `<PATH>` and make it executable.

### Usage

```
# $ um -h

um (-a|-d|-e|-h|-m|-r|-v) [<OPT>...]
um [<OPT>...] [<HOOK>]

SUBCOMMANDS
-----------
    ACTION                      OPT
    ------                      ---
    -a, --add                   -4,-7,-D,-H,-N,-U
    -d, --delete, --del         -0,-1,-2,-3,-7,-A,-D,-H,-M,-N,-n,-!,-0,
                                -P,-S,-U,-Y
    -e, --edit                  -0,-1,-2,-3,-7,-A,-D,-H,-M,-N,-!,-0,-P,
                                -S,-U,-Y
    -h, --help
    -m, --modify, --modi        -0,-1,-2,-3,-4,-5,-6,-7,-8,-9,-A,-D,-H,
                                -M,-N,-n,-!,-0,-P,-S,-U,-Y
    -r, --rebuild, --reb        -0,-1,-2,-3,-7,-A,-D,-H,-M,-N,-n,-!,-0,
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
    -7, --reference=, --ref=    <REF>
    -8, --reference+=, --ref+=  <REF>
    -9, --reference-=, --ref-=  <REF>
    -A, --and
    -D, --description=, --desc= <DESC>
    -H, --hierarchy=, --hier=   <HIER>
    -M, --mod
    -N, --name=                 <NAME>
    -n, --non-interactive, --nv
    -!, --not
    -O, --or
    -P, --part=                 <PART>
    -S, --scheme=               <SCHEME>
    -U, --uri=                  <URI>
    -x, --index
    -Y, --authority=, --auth=   <AUTH>

ARGUMENTS
---------
    <AUTH>*                     'string'
    <BD>                        'date'* or range 'date;date'. Two special
                                values: 'first' and 'last'
    <DESC>*                     'string'
    <HIER>*                     '/foo/bar/n'
    <HOOK>                      'string' without space character
    <ID>*                       'int', 'int;int;n' or range 'int-int'
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

#### Syntax

Since `urimark`(1) works with `getopts` and a long commad line syntax, we are relatively free to choose our favourite spelling. We can not only write all long subcommands and options without double hypen (`--`): instead of `--add` and `--id=` we may use `add` resp. `id=`; but also mix the syntaxes. The following means the same with different spelling:

```bash
$ um -0 42720785481233211538 -A -Y github.com -! -1 2
$ um --uuid=42720785481233211538 --and --authority=github.com --not --id=2
$ um --uuid=42720785481233211538 --and --auth=github.com --not --id=2
$ um uuid=42720785481233211538 and authority=github.com not id=2
$ um uuid=42720785481233211538 and auth=github.com not id=2
$ um uuid=42720785481233211538 -A -Y github.com -! id=2
```

Unlike GNU-like syntax, we can not write:

```bash
$ um -042720785481233211538 -A -Ygithub.com -! -12
```

But we may change the order of arguments when using subcommands, for example:

```bash
$ um add uri=https://github.com/ tag="git;web;code;collaboration;vcs"
$ um tag="git;web;code;collaboration;vcs" add uri=https://github.com/
```

And split up some options:

```bash
$ um add uri=https://github.com/ tag="git;web" tag="code;collaboration" tag=vcs
$ um id="first;2" id=last
$ um id="first;2" or id=last
$ um id="first" or id="2" or id=last
```

#### Adding

To add records to our database, we need at least to specify an URI. There are two ways. The first method is interactive adding in a tty with a simple `um add`; when we want to specify more than one tag or reference, we need then to separate them with semicolon (`;`). The second way could look like this:

```bash
$ um add \
    uri=https://bitbucket.org \
    name="Plant your code in the cloud. Watch it grow." \
    desc="web-based hosting service for projects that use either the Mercurial or Git revision control systems." \
    hier="/software/internet/web/crc" \
    tag="hg;mercurial;git;web;code;collaboration;vcs;atlassian" \
    ref=2
New authority has been recorded at file:///home/user/share/urimark/data/9535008412671455610 .
New URI with id 1 has been recorded.
Index is beeing rebuild...
Done.
```

If we try to add an existing URI, `urimark`(1) exits without recording:

```bash
$ um add uri=https://bitbucket.org
URI 'https://bitbucket.org' has already been recorded.
```

If there is already a record with the same authority, we get:

```bash
$ um add uri=https://bitbucket.org/features
Authority has already been recorded at file:///home/user/share/urimark/data/9535008412671455610 .
New URI with id 4 has been recorded.
Index is beeing rebuild...
Done.
```

Every new URI will get an `id`. In doing so, `urimark`(1) tries to assign the next number by looking for gaps in the sequence of ids:

```bash
$ um
42720785481233211538 2
42720785481233211538 3
9535008412671455610 1
9535008412671455610 4
$ um del -n id=1
Records have been deleted.
Index is beeing rebuild...
Done.
$ um
42720785481233211538 2
42720785481233211538 3
9535008412671455610 4
$ um add uri=https://bitbucket.org
Authority has already been recorded at file:///home/user/share/urimark/data/9535008412671455610 .
New URI with id 1 has been recorded.
Index is beeing rebuild...
Done.
$ um
42720785481233211538 2
42720785481233211538 3
9535008412671455610 1
9535008412671455610 4
```

If we do not specify all allowed metadata, the record will be filled with placeholders, which may be declared in the configuration file:

```bash
$ um id=1 info
UUID       9535008412671455610
ID         1
BD         1408309794
MD         1408309794
SCHEME     https
AUTHORITY  bitbucket.org
PART       /
NAME       null
DESC       null
HIER       null
TAGS       null
REF        null
```

#### Filtering

To have access to the database, there are the functions `__um_query()`, `__um_query_binary()` and `__um_query_post()`. A query will be initialized with filters and then manipulated with hooks. Filtering is the way to select records; hooking is the method to postprocess (cutting, piping, redirecting etc.) Internally, `urimarks` (1) uses only one declared hook. If there is no configuration in the conf file and specification on command line, this default hook will also be used to print all records and cut the `uuid` and `id` fields of them. Strictly speaking, a simple `um` is the same like `um um_default`: a hook, that acts like a report with no specified filter.

The available filters are these command line options: `-0,-1,-2,-3,-4,-7,-A,-D,-H,-N,-!,-O,-P,-S,-U,-Y`. They have following characteristics:
* Most of them are regextyped (posix-egrep)
* `id`, `bd`, `md`, `tag` and `reference` differ from the others because of the syntax of their values
* To have basic search possibilities, we have the operators: `and`, `or` and `not`
* Filters on command line may overwright filters, which are associated with hooks

Some basic filtering would be:

```bash
$ um scheme=https
$ um auth=^github.com$
$ um part=^/$
$ um uri=https://github.com/blog
$ um hierarchy=/software/internet$
$ um uuid=42712582202233536417
```

Filtering with IDs:

```bash
$ um id=^1$
$ um id=^1$\;^3$
$ um id=^1$\;^2$\;^3$
$ um id=^1$\-^3$
$ um id=^first$\-^last$
```

Filtering with dates (expressions of `date`(1) with option `--date` may be used):

```bash
$ um bd=^first$
$ um bd=^@1408102469$
$ um bd=^2014-08-17$
$ um bd=yesterday\;today
```

Filtering with tags:

```bash
$ um tag=^code$
$ um tag=^cod
$ um tag=^cod;web
```

Filtering in combination with operators:

```bash
$ um not authority=^bitbucket not part=^/$
$ um bd=yesterday\;today and authority=^github not part=^/blog$
$ um uuid=42720785481233211538 and bd=@1408102469 and ref=^2$
$ um id=^1$ id=^2$-^4$
$ um id=^1$ or id=^2$\-^4$ not id=^2$\;^3$
```

#### Editing

The subommand `edit` is the way to filter some records and edit them with our favourite editor. Therefore, `urimarks`(1) copies the specified id files into `<URIMARK_TMP_DIR>`; after editing, the modified file will be rewritten, the `md` field updated and all indexes rebuild.

```bash
$ um edit uuid=42720785481233211538
Data set with id 2 has not been modified.
Data set with id 3 has been modified.
Index is beeing rebuild...
Done.
```

#### Modifying

The subommand `modify` is our method to edit and update records without using an editor. `urimark`(1) directly modifies id files without copying them to `<URIMARK_TMP_DIR>`. After modifying, not the hole modified file will be rewritten (only the specified lines). With `modify` the filters `-5,-6,-8,-9` and the operator `mod` are available.

For example, to remove all tags with the value `code`, we could use:

```bash
$ um modify uuid=. mod tag-=code
UUID: 9535008412671455610
ID:   1
Do you really wanne modify this record? (y/n/all/quit) y
UUID: 42720785481233211538
ID:   2
Do you really wanne modify this record? (y/n/all/quit) y
UUID: 42720785481233211538
ID:   3
Do you really wanne modify this record? (y/n/all/quit) y
UUID: 9535008412671455610
ID:   4
Do you really wanne modify this record? (y/n/all/quit) y
Records have been modified.
Index is beeing rebuild...
Done.
```

We can add tags without prompting like this:

```bash
$ um modi -n id=^1$\;^2$ mod tag+=code
Records have been modified.
Index is beeing rebuild...
Done.
```

If we specify `tag` or `reference` as filter, we can change a hole value instead removing/adding:

```bash
$ um modi -n ref=^null$ mod ref=1
Records have been modified.
Index is beeing rebuild...
Done.
$ um id=4 _meta
4  null  null  null  null  1
$ um modi -n name=null and desc=null and hier=null and tag=null \
    mod tag=code name=bitbucket desc=empty hier=/software/internet/web/crc
Records have been modified.
Index is beeing rebuild...
Done.
$ um id=4 _meta
4  bitbucket  empty  /software/internet/web/crc  code  1
```

#### Deleting

```bash
$ um del id=4
UUID: 9535008412671455610
ID:   4
Do you really wanne delete this record? (y/n/all/quit) all
Records have been deleted.
Index is beeing rebuild...
Done.
```

#### Rebuilding

As we have seen, the indexes of our databases will be rebuild automatically. The subcommand `rebuild` should only be used, if we need to fix some self-inflicted failures. For examples:

To rebuild all id files of filterd `uuids`:

```bash
$ um rebuild auth=github.com # index => id files
UUID: 42720785481233211538
Do you really wanne rebuild all records with this uuid? (y/n/all/quit) all
IDs are beeing rebuild...
Done.
```

To rebuild the index files of filterd `uuids`:

```bash
$ um rebuild index auth=github.com # id files => index
UUID: 42720785481233211538
Do you really wanne rebuild all records with this uuid? (y/n/all/quit) y
Index is beeing rebuild...
Done.
```

If we are sure of corruption in `<${URIMARK_DATA_DIR}/_index>`, we need to take a filter with an `uuid` field. In conjunction with `rebuild` `urimark`(1) will use `find` instead `grep` to query our records:

```bash
$ um rebuild -n index uuid=42720785481233211538
$ um rebuild -n uuid=42720785481233211538
```

### Storage

Currently, `urimark`(1) can handle URLs with these protocols: `http`, `https`, `ftp`, `ftps`, `dav`, `davs`, `gopher`, `webdav`, `webdavs`. To store a record, an URL will be split up into the three separate fields `scheme`, `authority` and `part`. All records with the same `authority` share the same `uuid`; but every URL has its own line counted `id`. A complete record is constructed like in this example database with three data sets:

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

The storage backend is a combination of comma-separated values (`CSV`) and `bash` parameters, hierarchically arranged in our file system:

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

Along with this programme comes an exemplary [configuration file](../master/doc/examples/urimark.conf). The configurations will be sourced after command line parsing. We can set following parameters:

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

A hook is a set of connected subscripts of an associative array called `hook`; a valid hook needs to have a name (string without space character) and a description. Hooks come into play, when there is no regular subcommand on command line. If a hook has no specified filter, the filter on the command line will be used (`um [<FILTER>] <HOOK>`; `<FILTER>`: `-0,-1,-2,-3,-4,-7,-A,-D,-H,-N,-!,-O,-P,-S,-U,-Y`). Hooks will be called in the function `__um_query_post()`:

```bash
[[ ${hook[${hook_chosen} header]} ]] && eval "${hook[${hook_chosen} header]}"
eval "${hook[${hook_chosen} preprocess]} __um_query_post_result ${hook[${hook_chosen} postprocess]}"
[[ ${hook[${hook_chosen} footer]} ]] && eval "${hook[${hook_chosen} footer]}"
```

Since hooks work with `eval`, we need to take care about the correct quoting. But it is also possible to declare functions in the configuration file and to use them as command in the value of the arrays. In that case, we can proceed as usual.

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

### TODO

See file [TODO](../master/doc/TODO), which comes along with this programme.

### Bugs & Requests

Report it on https://github.com/D630/urimark
