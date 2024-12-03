cmake_minimum_required(VERSION 3.5)

project(pystring VERSION 1.1.4 )

option (BUILD_SHARED_LIBS "Build shared libraries (set to OFF to build static libs)" OFF)

add_library(pystring
    pystring.cpp
    pystring.h
)

set_target_properties(pystring PROPERTIES 
    PUBLIC_HEADER pystring.h
)

target_include_directories(
        pystring
        PUBLIC
        $<INSTALL_INTERFACE:include>
    )

add_executable (pystring_test test.cpp)
TARGET_LINK_LIBRARIES (pystring_test pystring)

enable_testing()
add_test(NAME PyStringTest COMMAND pystring_test)

include(GNUInstallDirs)

install(TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION include/pystring
)


include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY AnyNewerVersion
)

install(EXPORT ${PROJECT_NAME}
  FILE
  ${PROJECT_NAME}Config.cmake
  NAMESPACE
    ${PROJECT_NAME}::
  DESTINATION
    cmake
)

install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
  DESTINATION
    cmake
)

