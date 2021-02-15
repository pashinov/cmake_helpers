add_library(external_boost INTERFACE IMPORTED GLOBAL)
add_library(desktop-app::external_boost ALIAS external_boost)

if (MSVC)
    set(Boost_DIR "${libs_loc}/boost/lib/cmake/Boost-1.75.0")
endif()
set(Boost_USE_STATIC_LIBS ON)
find_package(Boost 1.75.0 CONFIG COMPONENTS system REQUIRED)

if (NOT ${Boost_FOUND})
    message(FATAL_ERROR "Boost couldn't be located")
endif()

target_link_libraries(external_boost INTERFACE Boost::system)
