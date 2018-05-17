If you are a packager for a Distribution not listed here, please take the time and create a raylib package.

# openSUSE
openSUSE has raylib in the official repository. Install it via:

```
zypper in raylib-devel
```

Users of older openSUSE versions will have to add the [devel:libraries:c_c++ devel project](https://build.opensuse.org/project/show/devel:libraries:c_c++).

Add the repo, refresh sources and install raylib:
```
zypper ar -f http://download.opensuse.org/repositories/devel:/libraries:/c_c++/openSUSE_Factory/devel:libraries:c_c++.repo
zypper ref
zypper in raylib-devel
```