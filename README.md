```
um (-A|-d|-e|-h|-m|-v)

SUBCOMMANDS
-----------
    ACTION                      OPT
    ------                      ---
    -A, --add                   -1,-2,-5,-8,-H,-N
    -d, --delete                -0,-1,-2,-5,-8,-!,-a,-B,-H,-I,-M,-N,-n,
                                -o,-S,-T,-Y
    -e, --edit                  -0,-1,-2,-5,-8,-!,-a,-B,-H,-I,-M,-N,-o,
                                -S,-T,-Y
    -h, --help
    -m, --modify                -0,-1,-2,-3,-4,-5,-6,-7,-8,-9,-!,-a,-B,
                                -H,-I,-M,-N,-n,-o,-S,-T,-Y
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
    -H, --hierarchy=            <HIER>
    -I, --id=                   <ID>
    -M, --md=                   <MD>
    -N, --name=                 <NAME>
    -n, --non-interactive
    -!, --not
    -o, --or
    -S, --scheme=               <SCHEME>
    -T, --part=                 <PART>
    -Y, --authority=            <AUTH>

ARGUMENTS
---------
    <AUTH>*                     'string'
    <BD>*                       'date' or 'date,date'
    <DESC>*                     'string'
    <HIER>*                     '/foo/bar'
    <ID>*                       'int', 'int,int', 'int-int' (or combi).
                                Two special values: 'first' and 'last'.
    <MD>*                       'date' or 'date,date'
    <NAME>*                     'string'
    <PART>*                     'string'
    <REF>*                      'int' or 'int,int'
    <SCHEME>*                   'http','https','ftp','ftps','dav','davs',
                                'gopher','webdav','webdavs
    <TAG>*                      'string' or 'string;string;string'
    <URI>                       'string'
    <UUID>*                     'uuid'

    *regextype: posix-egrep
```
