sample autoenv to include all npm binaries from the local node_modules

use autoenv fork from:
https://github.com/ijcd/autoenv.git

.env
```
#!/bin/bash
CWD=${0%/*}
OLDPATH=$PATH
autoenv_info_msg=%{$fg[cyan]%}'[autoenv: custom nodepath] '

PATH=${CWD}/node_modules/.bin:$PATH;

unset CWD
```


.unenv
```
#!/bin/bash
PATH=$OLDPATH
unset OLDPATH
unset autoenv_info_msg
```
