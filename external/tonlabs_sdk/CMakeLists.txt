add_library(external_tonlabs_sdk INTERFACE IMPORTED GLOBAL)
add_library(desktop-app::external_tonlabs_sdk ALIAS external_tonlabs_sdk)

set(sdk_loc ${libs_loc}/TON-SDK)
set(sdk_build_loc ${sdk_loc}/target/i686-pc-windows-msvc/release)
if (WIN32)
    set(sdk_build_suffix .lib)
else()
    set(sdk_build_suffix .a)
endif()

target_include_directories(external_tonlabs_sdk
INTERFACE
    ${sdk_loc}/ton_client/client
)

target_link_libraries(external_tonlabs_sdk
INTERFACE
        "${sdk_build_loc}/ton_client.dll.lib"
)
