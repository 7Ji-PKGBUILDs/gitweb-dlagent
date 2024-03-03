# Archlinux Gitweb DLAGENT
## What is it?

This is a dlagent, where git vcs sources can be downloaded in PKGBUILD without cloning the whole git tree. It uses web scraping techniques to detect what is the latest commit and revision info for a given vcs url in PKGBUILD, downloads that tar ball, caches it, and updates the source tree when necessary.

## How to use it?

1 - Register the dlagent in PKGBUILD as below

```shell
DLAGENTS+=('gitweb-dlagent::/usr/bin/gitweb-dlagent sync %u')
```

2 - Change the url scheme in source to `gitweb-dlagent://`

```shell
source=("gitweb-dlagent://github.com/hbiyik/agr/#branch=master"
```

3 - Get the revision/commit info with `gitweb-dlagent` if pkgver uses such facility

```shell
pkgver(){
  # to get version with rNUMCOMMITS.COMMITHASH
  local _revision=$(gitweb-dlagent version gitweb-dlagent://github.com/hbiyik/agr/#branch=master)
  # to get revision only
  local _revision=$(gitweb-dlagent version gitweb-dlagent://github.com/hbiyik/agr/#branch=master --pattern \{revision\})
  # to get commit only
  local _revision=$(gitweb-dlagent version gitweb-dlagent://github.com/hbiyik/agr/#branch=master --pattern \{commit:.10s\})
  
  # in all above cases a http get request to github is made, if you want to directly use
  # the cached revision and commit info, they are stored under .gitweb/{reponame}_revison
  # and .gitweb/{reponame}_commit files, so you can cat them as below
  local _cached_commit $(cat ${srcdir}/../.gitweb/${repo}_commit)
  local _cached_revision $(cat ${srcdir}/../.gitweb/${repo}_revision)
 } 
```

## Limitations

1 - Currently only github is supported, but similarly gitlab or any other service can be extended, script is written modular

2 - It is not possible to get signed tar balls since currently there is no established way of getting signed tar balls.

3 - Revision and commit info can only get received for the root of the git directory. Path specific revision info can not be received over web interface.

4 - Script does not use any api, therefore there is no rate limit problems. However the parsing of revision and commit is done through regex and sometimes due to web site layout updates, regex definitions may need future maintenance. They are written error and change tolerant so this should not be a frequent case, by experience once in each 3 or 4 years.  
