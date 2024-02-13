A Deep Dive into V8 Sandbox Escape Technique Used in In-The-Wild Exploit https://blog.theori.io/a-deep-dive-into-v8-sandbox-escape-technique-used-in-in-the-wild-exploit-d5dcf30681d4

Javascript exploit code: https://github.com/theori-io/v8-sbx-bypass-wasm/tree/main

v8 build instructions: https://chromium.googlesource.com/external/github.com/v8/v8.wiki/+/8c0be5e888bda68437f15e2ea9e317fd6229a5e3/Building-with-GN.md

system prep

        sudo apt update
        sudo apt -y upgrade
        sudo apt -y install git build-essential pkg-config

get google tools

        git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
        export PATH=~/depot_tools:$PATH

get source

        cd ~
        fetch v8
        mv v8 v8-11-4-0
        cd v8-11-4-0
        git checkout f7a3499f6d7e50b227a17d2bbd96e4b59a261d3c
        gclient sync
        cd v8/src

configure for build

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

build

        autoninja -C out/x64.release

Run

        ~/v8-11-4-0/out/x64-release/d8 e[+] instance_addr = 0x19c3d8

Result gives you a shell

                [+] tbl_addr = 0x4dc04
                [+] exported_func_addr = 0x19c55c
                [+] imported_function_targets = 0x4def1
                [+] rwx = 0x39e47ef46700
                [+] indirect_function_tables = 0x4df4d
                [+] indirect_function_table = 0x4df69
                $ 




        
