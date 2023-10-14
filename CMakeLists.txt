# Minimum version for cmake compatiblity
cmake_minimum_required(VERSION 3.27)

include(CMakePrintHelpers)
include(FetchContent)

# Set branch names for the sub modules dependencies
set(GITHUB_BRANCH_TOOLCHAIN "HEAD" CACHE STRING "git SHA for CMake Toolchain")
# show limited messages while cloning git repos
set(FETCHCONTENT_QUIET ON)

cmake_print_variables(GITHUB_BRANCH_TOOLCHAIN)
FetchContent_Declare(
    cmake-toolchains                             # Recommendation: Stick close to the original name.
    GIT_REPOSITORY  git@github.com:kodezine/cmake-toolchains.git
    GIT_TAG         ${GITHUB_BRANCH_TOOLCHAIN}
)

# pre-fetch the toolchain repository as the first job before project configuration
FetchContent_GetProperties(cmake-toolchains)

if(NOT cmake-toolchains_POPULATED)
    FetchContent_Populate(cmake-toolchains)
endif()

set(GITHUB_BRANCH_cmsis-v5 "5.9.0")# CACHE STRING "Verson for the cmsis")

project(
    cmsis-v5
    VERSION ${GITHUB_BRANCH_cmsis-v5}
    LANGUAGES C
    DESCRIPTION "Header only library for CMSIS v5"
)

include(${cmake-toolchains_SOURCE_DIR}/frameworks/arm_${PROJECT_NAME}/arm_${PROJECT_NAME}.cmake)

# This will set the CPACK tar file as
# cmsis-v5-<cmsisVersion>-<cortexType>-<compiler>-<compilerVersion>.tar.gz
set(CPACK_PACKAGE_CHECKSUM SHA3_256)
set(CPACK_SYSTEM_NAME "$ENV{CORTEX_TYPE}-${CMAKE_C_COMPILER_ID}-${CMAKE_C_COMPILER_VERSION}")
set(CPACK_BINARY_TGZ "ON")
set(CPACK_BINARY_ZIP "OFF")
set(CPACK_BINARY_ZIP "OFF")
set(CPACK_BINARY_NSIS "OFF")
set(CPACK_SOURCE_IGNORE_FILES
  \\.git/
  build/
  ".*~$"
)
set(CPACK_VERBATIM_VARIABLES YES)
include(CPack)
