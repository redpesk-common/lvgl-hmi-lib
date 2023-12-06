Pre-build config for Tux-evse lvgl lib

### Clone tux-evse-lvgl + (lvgl-lib + lvgl-drivers submodules)
```
git clone git@github.com:tux-evse/lv-evse-hmi-lib.git
cd lv-evse-hmi-lib
git submodule init
git submodule update
```

### for frame-buffer use
no special dependencies required

```
mkdir build
cd build
cmake ..
make -j install
```

### for gtl emulator

install gtk dependencies

```
sudo dnf install -y gtk3-devel
```

compile with GTK_USE option

```
mkdir build
cd build
cmake -DUSE_GTK=1 -DX_RES=1024 Y_RES=600 ..
make -j install
```

## Warning

GTK seems to break json-c dynamic loading order. You should enforce the lib at binding level to enforce json-c original API.
```
fn main() {
    println!("cargo:rustc-link-search=/usr/local/lib64");
    println!("cargo:rustc-link-arg=-ljson-c");
    println!("cargo:rustc-link-arg=-llvgl");
    println!("cargo:rustc-link-arg=-llv_drivers");
}
```

I had to manually change in lvgl to get a dynamic lib
```
//Build shared as library (as opposed to static)
BUILD_SHARED_LIBS:BOOL=ON
```