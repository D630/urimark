# NAME

urimark - maintain a list of bookmarks on command line.

# SYNOPSIS

    um (-h|-v| )
        (-a|-c|-d|-D|-e|-l|-m|-r|-s|-t|-i)
        [-A|-n|-u|-T|-f|-p]

# REQUIEREMENT

-   GNU bash >= 4.0
-   GNU find
-   GNU sed
-   GNU xargs
-   cp, cut, date, grep, md5sum, mkdir, mktemp, od, rm, sort, tail, tr, tsort, uniq, wc

# DESCRIPTION

TODO

# HELP

    USAGE
    -----
    um (-h|-v| )
        (-a|-c|-d|-D|-e|-l|-m|-r|-s|-t|-i)
        [-A|-n|-u|-T|-f|-p]

    OPTIONS
    -------
        -h,  -help
        -v,  -version

    SUBCOMMANDS
    -----------
        ACTION                  OPTION          ARG
        ------                  ------          --------
        -a, -add                                [ <AFIELD> ... ]
        -c, -cloud                              <CFIELD>
        -d, -delete             -n              [ <EXP> ... ]
        -D, -dep                -u, -T , -f     <DFIELD>
        -e, -edit                               <EEXP> ...
        -l, -list               -u              [ <EXP> ... ]
        -m, -modify             -n              [ <EXP> ... ] 'MOD' <MEXP> ...
        -r, -rebuild                            [ <EXP> ... ]
        -s, -search             -A              [ <EXP> ... ]
        -t, -tags               -p              [ <EXP> ... ]
        -i, -info

        OPTION                  ARG
        ------                  ---
        -A, -absolute-path
        -n, -non-interactive
        -u, -print-uri
        -T, -specific-tree
        -f, -follow-note
        -p, -print-tags

    ARGUMENTS
    ---------
        <AFIELD>    'uri:string', 'name:string', 'description:string',
                    'hierarchy:/foo/foo', 'tag:"foo;foo;foo"' or
                    'reference:int,int,int'.
        <CFIELD>    'scheme', 'authority', 'part', 'hierarchy', 'tag' or
                    'reference'.
        <DFIELD>*   ID or Reference number specified by an integer.
        <EXP>
                    <UUID>*     'uuid:uuid'
                    <ID>*       'id:int', 'id:int,int', 'id:int-int' (or
                                combination).
                    <URI>       'uri:string'
                    <AUTH>*     'authority:string'
                    <SCHEME>*   'scheme:string'
                    <PART>*     'part:string'
                    <MD>*       'md:date' or 'md:date,date'.
                    <BD>*       'bd:date' or 'md:date,date'
                    <NAME>*     'name:string'
                    <DESC>*     'description:string'
                    <HIER>*     'hierarchy:/foo/foo'
                    <TAG>*      'tag:"foo;foo;foo"'
                    <REF>*      'reference:int' or 'reference:int,int'.
                    <AND>       and
                    <OR>        or
                    <NOT>       not
        <EEXP>*     <UUID> or <ID>.
        <MEXP>      'authority:string', 'scheme:string', 'part:string',
                    'name:string', 'description:string',
                    'hierarchy:/foo/foo', 'tag:string', 'tag-:string',
                    'tag+:string', 'reference:reference',
                    'reference-:reference' or 'reference+:reference'

        *regextype: posix-egrep

# EXAMPLES

## add

You can add a record by writing the subcommand 'add' without any arguments. The
bash prompts you then to record all used fields:

    um add

But it is more useful to give it an argument. All records need at least one
'uri' field. So you need to indicate minimum:

    um add uri:"https://github.com/D630/urimark"

A fully formulated command could be look like this:

    um add uri:"https://github.com/D630/urimark" name:urimark description:"maintain
    a list of urimarks aka. bookmarks"
    hierarchy:/software/internet/bookmarking/urimark
    tag:"bookmarks;github;bash;command line" reference:1,100,1000

## search

The 'search' subcommand is special, because most other commands use it to match
a pattern. But you can access on it by:

    um search
    um search -A

Above commands would print all records. The first shows the 'uuid' and 'id' from
every record; the second would print an absolute pathname and as second field
the 'id'. The search pattern is regextyped (posix-egrep). See section "Help" for
details, which command can be used with a pattern. So, a simple search for the
record with "id=1" could be:

    um search -A id:^1$

The pattern needs to have one of the following labels as prefix:

-   uuid:
-   id:
-   authority:
-   scheme:
-   part:
-   md:
-   bd:
-   name:
-   description:
-   hierarchy:
-   tag:
-   reference:

Special labels are 'id', 'md' (modification date) and 'bd' (build date). You can
use a id-pattern like this:

    um search id:^1$ id:^2$ id:^3$ id:^4$ id:^5$
    um search id:^1\-^5$
    um search id:^1$,^5$

To specify a 'md' or 'bd' pattern, you can indicate a date or a period:

    um search bd:2014-01-01
    um search bd:2014-01-01,2014-04-01

Try the time specification in a format, which is provided by the date(1)
command.

To have very basic search operators you can specify the search with 'or' 'and'
and 'not'. Following command

    um search id:^1\-^500$ scheme:^http$ part:git

is the same as:

    um search id:^1\-^500$ or scheme:^http$ or part:git

You can modfiy it with:

    um search id:^1\-^500$ and scheme:^http$ and part:git
    um search id:^1\-^500$ and part:git not scheme:^https$

## edit

To edit/manipulate a record in a terminal, you can specify it with its 'id' or
'uuid':

    um edit id:^1$
    um edit uuid:10722768586838226331880532394357803772

To edit one after another:

    um edit id:^1$ id:^5$
    um edit id:^1$,^5$
    um edit id:^1$\-^5$
    um edit uuid:10722768586838226331880532394357803772 uuid:14927278103552633479203832032284475425

## list

The 'list' subcommand gives a output in a CVS-format. Without any pattern all
records will be printed. The option '-u' indicates 'urimark' to build an uri-field
instead of three fields (scheme, authority, part):

    um list -u
    um list -u id:^1$\-^100$
    um list -u id:^1$\-^100$ | urlview

## delete

To delete all records without prompting before every removal (you would be asked
"y,no,all,quit").

    um delete -n

To delet records with id one to ten:

    um delete id:^1$\-^10$

## modify

To modify one or more records, you need to indicate a pattern and a
modification/replacement. To add/remove a tag to/from every record:

    um modify MOD tag+:urimark
    um modify MOD tag-:urimark

To tag only records with the scheme "https":

    um modify scheme:^https$ MOD tag+:ssl

When you use a "tag" or "reference" label as pattern prefix, you can replace the
whole field. For example, to replace all tags "ssl" with "secret"

    um modfiy tag:^ssl$ MOD tag:secret

## tags

To get a list with uri, id and tag fields for example:

    um tags -p id:^1$\-^10$

## dep

The 'dep' subcommand prints dependencies between the fields 'id' and 'reference'
in an "outline view". To show all children of an specfic 'id':

    um dep -T -u ^1$

If a children has children, too, write:

    um dep -T -u -f ^1$

To outline all records, which are referenced with an 'id':

    um dep -u ^1$

## cloud

To inform about frequency and print a "cloud" for all tags:

    um cloud tag

## rebuild

To rebuild the index of a record, which matches a pattern:

    um rebuild id:^1$

## info

    um info

# NOTES

-   You may write all subcommands and options without masking '-'. So,
    instead of '-help' you may use 'help'.
-   Currently, 'scheme' field needs to be one of this: http, https, ftp, ftps, dav,
    davs, gopher, webdav, webdavs.

# ENVIROMENT

    - To locate data dir the varibale 'URIMARK_DATA' is used. If this is not set,
      'XDG_DATA_HOME' respec. '${HOME}/.local/share' will be used instead.
    - For the command 'um edit' variable 'EDITOR' is used, instead 'nano' will be
      invoked. Additionally, this command uses 'TMPDIR' respec. '/tmp', where it
      creates a tmp file.

# BUGS & REQUESTS

Report it on 'https://github.com/D630/urimark'.

# TODO

See file 'TODO', which comes along with this programm.

# LICENSE

'urimark' is licensed with 'GNU GPLv3'. You should have received a copy of the
'GNU General Public License' along with this program. If not, see for more
details '<http://www.gnu.org/licenses/gpl-3.0.html>'.

# CHRONOLOGY

First version ('0.1.0.0') was finished on: 18. April 2014.
