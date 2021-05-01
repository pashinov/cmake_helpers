add_library(external_hidapi INTERFACE IMPORTED GLOBAL)
add_library(desktop-app::external_hidapi ALIAS external_hidapi)

find_package(PkgConfig REQUIRED)

set(hidapi_name)
if (WIN32)
elseif (UNIX AND NOT APPLE)
    set(hidapi_name hidapi-libusb)
elseif (APPLE)
    set(hidapi_name hidapi)
endif()
pkg_check_modules(HIDAPI REQUIRED ${hidapi_name})
target_include_directories(external_hidapi INTERFACE ${HIDAPI_INCLUDE_DIRS})
target_link_libraries(external_hidapi INTERFACE
        ${HIDAPI_LIBDIR}/${CMAKE_STATIC_LIBRARY_PREFIX}${HIDAPI_LIBRARIES}${CMAKE_STATIC_LIBRARY_SUFFIX})


pkg_check_modules(USB-1.0 REQUIRED libusb-1.0)
target_include_directories(external_hidapi INTERFACE ${USB-1.0_INCLUDE_DIRS})
if (WIN32)
elseif (UNIX AND NOT APPLE) # libusb dynamic linkling
    target_link_libraries(external_hidapi INTERFACE ${USB-1.0_LIBRARIES})
elseif (APPLE) # libusb static linkling
    target_link_libraries(external_hidapi INTERFACE
            ${USB-1.0_LIBDIR}/${CMAKE_STATIC_LIBRARY_PREFIX}${USB-1.0_LIBRARIES}${CMAKE_STATIC_LIBRARY_SUFFIX})
endif()
