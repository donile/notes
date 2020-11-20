# apt-get

## The following packages have been kept back
If the dependencies have changed on one of the packages you have installed so that a new package must be installed to perform the upgrade then that will be listed as "kept-back".

`> sudo apt-get --with-new-pkgs upgrade`

```
--with-new-pkgs
    Allow installing new packages when used in conjunction with upgrade. This is useful if the update of an
    installed package requires new dependencies to be installed. Instead of holding the package back upgrade
    will upgrade the package and install the new dependencies. Note that upgrade with this option will never
    remove packages, only allow adding new ones. Configuration Item: APT::Get::Upgrade-Allow-New.
```