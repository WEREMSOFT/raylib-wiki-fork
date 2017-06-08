openSUSE Tumbleweed has raylib in the [devel:libraries:c_c++ devel project](https://build.opensuse.org/project/show/devel:libraries:c_c++).

You can easily install it by adding the repository, refreshing your sources and insalling:
```
zypper ar -f http://download.opensuse.org/repositories/devel:/libraries:/c_c++/openSUSE_Factory/devel:libraries:c_c++.repo
zypper ref
zypper in raylib-devel
```

If you are a packager for another Distribution, please take the time and create a raylib package.