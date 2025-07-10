# Metabase Doris Driver

The Metabase Doris driver allows Metabase v0.48.0 or above to connect to Doris databases.If Metabase version below v0.48.0,try to use mysql driver  to connection with doris.

## Function
1. synch doris database tables infomation
2. synch doris table fields infomation
3. query data from doris with sql  

## Downloading and Installing Doris Driver

### Downloading Doris Metabase Driver

[Click here](https://github.com/ihadoop/metabase-doris-driver/releases) to view the latest release of the Metabase Doris driver; click the link to download `doris.metabase-driver.jar`.

### How to Install it

Metabase will automatically make the Doris driver if it finds the driver JAR in the Metabase plugins directory when it starts up.

Follow steps shown bellow to install the driver properly:
1. Create the directory (if it's not already there)
2. Move the Doris metabase driver JAR you just downloaded into it ("doris.metabase-driver.jar")
3. Restart Metabase

By default, the plugins directory is called `plugins`, and lives in the same directory as the Metabase JAR.

For example, if you're running Metabase from a directory called `/app/`, you should move the Doris driver JAR to `/app/plugins/`:

```bash
# example directory structure for running Metabase with Doris support
./metabase.jar
./plugins/doris.metabase-driver.jar
```

## Building the driver

## Prereq: Install the Clojure CLI

Make sure you have the `clojure` CLI version `1.11.1.1435` or newer installed; you can check this with `clojure
--version`. Follow the instructions at https://clojure.org/guides/getting_started if you need to install a
newer version.

## Prereq: Clone the Metabase core repo

```sh
git clone https://github.com/metabase/metabase
```

## Build it (Updated for build script changes in Metabase 0.48.0)

Unfortunately the current command for building a driver is quite a mouthful, but we needed to make changes to how the
Metabase build script works to avoid issues with dependencies shadowing one another. Please upvote
https://ask.clojure.org/index.php/7843/allow-specifying-aliases-coordinates-that-point-projects , which will allow us
to make building drivers much more convenient in the future!

```sh
# Example for building the driver with bash or similar

# switch to the local checkout of the Metabase repo
cd /path/to/metabase/repo

# get absolute path to the driver project directory
export DRIVER_PATH=`readlink -f ~/doris-driver`

# Build driver. See explanation below
clojure  -J-Dhttps.protocols=TLSv1.2,TLSv1.3   -Sdeps "{:aliases {:doris {:extra-deps {com.metabase/doris-driver {:local/root \"$DRIVER_PATH\"}}}}}"    -X:build:doris   build-drivers.build-driver/build-driver!   "{:driver :doris, :project-dir \"$DRIVER_PATH\", :target-dir \"$DRIVER_PATH/target\"}"
```

