#
# https://github.com/theori-io/v8-sbx-bypass-wasm/tree/main
# https://blog.theori.io/a-deep-dive-into-v8-sandbox-escape-technique-used-in-in-the-wild-exploit-d5dcf30681d4
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
cd v8
git checkout f7a3499f6d7e50b227a17d2bbd96e4b59a261d3c
gclient sync

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

# build
autoninja -C out/x64.release
