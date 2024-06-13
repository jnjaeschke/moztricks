# Using build cache

```sh
cd $HOME
git clone https://gitlab.com/bits-n-bites/buildcache.git
mkdir buildcache && cd buildcache
cmake -DCMAKE_BUILD_TYPE=Release ../src
cmake --build . --config Release
```

Add to `$PATH` in .zshrc:

```sh
export PATH=$HOME/buildcache/build/:$PATH;
```

Create `.buildcache_XXX` directories for each build config.
Add a config.json in each of them:

```json
{
  "compress": true,
  "max_cache_size": 10737418240,
  "max_local_entry_size": 268435456
}
```

(cache size: 10G, local entry size: 256M)

Add this to mozconfig, specifying the buildcache dir:

```sh
mk_add_options 'export BUILDCACHE_DIR=/home/USER/.buildcache_XXX'
mk_add_options 'export RUSTC_WRAPPER=buildcache'
ac_add_options --with-ccache=buildcache
```


In case cache needs to be purged:
```sh
buildcache -d /home/USER?buildcache_XXX -C
```