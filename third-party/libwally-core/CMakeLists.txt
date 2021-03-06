cmake_minimum_required (VERSION 2.8)

add_library(libwally-core
    src/aes.c
    src/base58.c
    src/bip32.c
    src/bip38.c
    src/bip39.c
    src/elements.c
    src/hex.c
    src/hmac.c
    src/internal.c
    src/mnemonic.c
    src/pbkdf2.c
    src/scrypt.c
    src/sign.c
    src/wordlist.c
    src/ccan/ccan/crypto/ripemd160/ripemd160.c
    src/ccan/ccan/crypto/sha256/sha256.c
    src/ccan/ccan/crypto/sha512/sha512.c
    src/ccan/ccan/str/hex/hex.c
)
target_include_directories(libwally-core
    PUBLIC
        .
        src
        src/ccan
        src/secp256k1
        src/secp256k1/include
)
target_link_libraries(libwally-core PUBLIC secp256k1)
target_compile_definitions(libwally-core PRIVATE SECP256K1_BUILD)

if (DEFINED IOS_PLATFORM)
    target_compile_options(libwally-core
        PRIVATE
            -W -Wcast-align -Wnested-externs -Wshadow -Wstrict-prototypes
            -Wformat -Wformat-security  -Wformat-nonliteral -O3
            -D_FORTIFY_SOURCE=1 -fstack-protector-strong -DNDEBUG=1 -flto
            -flax-vector-conversions -Wno-unused-function -Wno-long-long
            -Wno-overlength-strings -Wno-variadic-macros  -MP
    )
endif()

add_library(secp256k1
    src/secp256k1/src/secp256k1.c
)

target_include_directories(secp256k1
    PUBLIC
        src/secp256k1
        src/secp256k1/src
        src/secp256k1/include
)

target_compile_options(secp256k1
    PRIVATE
        -std=c89
)

target_compile_definitions(secp256k1
        PUBLIC
          -DHAVE_CONFIG_H
        PRIVATE
          -DENABLE_MODULE_RANGEPROOF -DENABLE_MODULE_SURJECTIONPROOF
          -DENABLE_MODULE_ECDH -DENABLE_MODULE_GENERATOR -DENABLE_MODULE_RECOVERY
          -DENABLE_MODULE_RECOVERY
)
