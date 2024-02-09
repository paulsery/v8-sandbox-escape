#
# https://github.com/theori-io/v8-sbx-bypass-wasm/tree/main
#


# system prep

sudo apt update

sudo apt -y upgrade

sudo apt -y install git build-essential pkg-config

# get google tools

git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git

export PATH=~/depot_tools:$PATH


# get source

cd ~
fetch v8
mv v8 v8-11-4-0
cd v8-11-4-0
git checkout f7a3499f6d7e50b227a17d2bbd96e4b59a261d3c
gclient sync
cd v8/src

# configure for build (args.gn => ~/v8/out/x64.release/args.gn)
gn args out/x64.release

        is_component_build = false
        is_debug = false
        target_cpu = "x64"
        v8_enable_sandbox = true
        use_goma = false
        v8_enable_backtrace = true
        v8_enable_disassembler = true
        v8_enable_object_print = true
        v8_enable_verify_heap = true
        dcheck_always_on = false
        v8_expose_memory_corruption_api = true

# build
autoninja -C out/x64.release
