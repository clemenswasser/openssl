cmake_minimum_required(VERSION 3.20)

project(OpenSSL LANGUAGES C ASM_NASM)

add_library(openssl_defaults INTERFACE)

if(MSVC)
    target_compile_options(openssl_defaults INTERFACE /W3 /wd4090)
endif()

target_include_directories(openssl_defaults INTERFACE
    "${OpenSSL_SOURCE_DIR}"
    "${OpenSSL_SOURCE_DIR}/include"
    "${OpenSSL_SOURCE_DIR}/providers/common/include"
    "${OpenSSL_SOURCE_DIR}/providers/implementations/include")

if(CMAKE_C_BYTE_ORDER STREQUAL "LITTLE_ENDIAN")
    target_compile_definitions(openssl_defaults INTERFACE L_ENDIAN)
else()
    target_compile_definitions(openssl_defaults INTERFACE B_ENDIAN)
endif()

if(WIN32)
    target_compile_definitions(openssl_defaults INTERFACE
        OPENSSL_SYS_WIN32
        WIN32_LEAN_AND_MEAN
        UNICODE
        _UNICODE
        _CRT_SECURE_NO_DEPRECATE
        _WINSOCK_DEPRECATED_NO_WARNINGS)
        target_link_libraries(openssl_defaults INTERFACE Ws2_32.lib Crypt32.lib)
endif()

target_compile_definitions(openssl_defaults INTERFACE
    OPENSSL_PIC
    "OPENSSLDIR=\"C:\\Program Files\\Common Files\\SSL\""
    "ENGINESDIR=\"C:\\Program Files\\OpenSSL\\lib\\engines-3\""
    "MODULESDIR=\"C:\\Program Files\\OpenSSL\\lib\\ossl-modules\""
    OPENSSL_BUILDING_OPENSSL
    $<$<NOT:$<CONFIG:Debug>>:NDEBUG>)

add_subdirectory(crypto)
add_subdirectory(providers)

include(CTest)

if(BUILD_TESTING)
    add_subdirectory(test)
endif()