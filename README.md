```
um (-A|-c|-d|-D|-e|-h|-i|-l|-m|-r|-s|-t|-v)

SUBCOMMANDS
-----------
    ACTION                      OPT
    ------                      ---
    -A, --add                   -1,-2,-5,-8,-H,-N
    -c, --cloud=<CFIELD>
    -d, --delete                -0,-1,-2,-5,-8,-!,-a,-B,-H,-I,-M,-N,-n,
                                -o,-S,-T,-Y
    -D, --dep=<DFIELD>          -E,-f,-u
    -e, --edit                  <EEXP>
    -h, --help
    -i, --info
    -l, --list                  -0,-1,-2,-5,-8,-!,-a,-B,-H,-I,-M,-N,-o,
                                -S,-T,-u,-Y
    -m, --modify                -0,-1,-2,-3,-4,-5,-6,-7,-8,-9,-!,-a,-B,
                                -H,-I,-M,-N,-n,-o,-S,-T,-Y
    -r, --rebuild               -0,-1,-2,-5,-8,-!,-a,-B,-H,-I,-M,-N,-o,
                                -S,-T,-Y
    -s, --search                -0,-1,-2,-5,-8,-!,-a,-B,-H,-I,-M,-N,-o,
                                -P,-S,-T,-u,-Y
    -t, --tags                  -0,-1,-2,-5,-8,-!,-a,-B,-H,-I,-M,-N,-o,
                                -p,-S,-T,-u,-Y
    -v, --version

    OPT                         ARG
    ---                         ---
    -0, --uuid=                 <UUID>
    -1, --uri=                  <URI>
    -2, --tag=                  <TAG>
    -3, --tag+=                 <TAG>
    -4, --tag-=                 <TAG>
    -5, --reference=            <REF>
    -6, --reference+=           <REF>
    -7, --reference-=           <REF>
    -8, --description=          <DESC>
    -9, --mod
    -a, --and
    -B, --bd=                   <BD>
    -E, --specific-tree
    -f, --follow-node
    -H, --hierarchy=            <HIER>
    -I, --id=                   <ID>
    -M, --md=                   <MD>
    -N, --name=                 <NAME>
    -n, --non-interactive
    -!, --not
    -o, --or
    -P, --absolute-path
    -p, --print-tags
    -S, --scheme=               <SCHEME>
    -T, --part=                 <PART>
    -u, --print-uri
    -Y, --authority=            <AUTH>

ARGUMENTS
---------
    <AUTH>*                     'string'
    <BD>*                       'date' or 'date,date'
    <CFIELD>                    'scheme', 'authority', 'part',
                                'hierarchy', 'tag' or 'reference'
    <DESC>*                     'string'
    <DFIELD>*                   id or reference number specified by
                                an integer (only one value; no range)
    <EEXP>*                     <UUID> or <ID>
    <HIER>*                     '/foo/bar'
    <ID>*                       'int', 'int,int', 'int-int' (or combi)
    <MD>*                       'date' or 'date,date'
    <NAME>*                     'string'
    <PART>*                     'string'
    <REF>*                      'int' or 'int,int'
    <SCHEME>''                  'http','https','ftp','ftps','dav','davs',
                                'gopher','webdav','webdavs
    <TAG>*                      'string' or 'string;string;string'
    <URI>                       'string'
    <UUID>*                     'uuid'

    *regextype: posix-egrep
```
