```
um (-a|-d|-e|-h|-m|-r|-v)

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
    <BD>                        'date'* or 'date;date'. Two special
                                values: 'first' and 'last'.
    <DESC>*                     'string'
    <HIER>*                     '/foo/bar'
    <ID>*                       'int', 'int,int', 'int-int' (or combi).
                                Two special values: 'first' and 'last'.
    <MD>                        'date'* or 'date;date'. Two special
                                values: 'first' and 'last'.
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
