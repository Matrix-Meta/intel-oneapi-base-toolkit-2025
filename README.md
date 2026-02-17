# Intel OneAPI Base Toolkit

OneAPI packages are distributed in binary form. Hence we need to fetch the prebuilt
packages from Intel's mirrors.

## Updating the Source Array

The installer for the base toolkit that contains all relevant packages of the
oneAPI stack can be obtained from
```
https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit-download.html
```
by selecting `Intel OneAPI Base Toolkit`, then `Linux`, followed by `Offline Installer`.
The download link is hidden behind a JS script that we can infer by clicking
on `Continue as a Guest`. From the URL we then extract the `_pkgmagic` and `_urlmagic`
variables for the `source` array in the `PKGBUILD` script.

## Nvchecker Integration

The `nvchecker` configuration uses Intel's apt repositories to check the version
of the basekit meta package. Note that we can't download the package directly
from the apt mirror, so `pkgctl version upgrade` doesn't work. Instead, follow
the instructions in the previous paragraph to update the `source` array.
