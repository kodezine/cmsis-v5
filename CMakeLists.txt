# Minimum version for cmake compatiblity
cmake_minimum_required(VERSION 3.27)

include(CMakePrintHelpers)
include(FetchContent)

# set the location of all fetched repos
set(FETCHCONTENT_BASE_DIR "${CMAKE_SOURCE_DIR}/_deps")
# Set branch names for the sub modules dependencies
set(GITHUB_BRANCH_cmake_scripts "HEAD" CACHE STRING "git SHA for CMake Toolchain")
# show limited messages while cloning git repos
set(FETCHCONTENT_QUIET ON)

cmake_print_variables(GITHUB_BRANCH_cmake_scripts)
FetchContent_Declare(
    cmake_scripts                             # Recommendation: Stick close to the original name.
    GIT_REPOSITORY  git@github.com:kodezine/cmake_scripts.git
    GIT_TAG         ${GITHUB_BRANCH_cmake_scripts}
)

# pre-fetch the toolchain repository as the first job before project configuration
FetchContent_MakeAvailable(cmake_scripts)

set(GITHUB_BRANCH_cmsis_v5 "5.9.0")

project(
    cmsis_v5
    VERSION ${GITHUB_BRANCH_cmsis_v5}
    LANGUAGES C
    DESCRIPTION "Header only library for CMSIS v5"
)

include(${cmake_scripts_SOURCE_DIR}/frameworks/cmsis_v5/cmsis_v5.cmake)
