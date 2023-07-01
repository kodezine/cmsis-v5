# Minimum version for cmake compatiblity
cmake_minimum_required(VERSION 3.25)
include(CMakePrintHelpers)
include(FetchContent)

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

project(libCMSIS)

# Set pre-requsities for selecting the CPU identifier for cross compilation
set(CORTEX_TYPE "cm7" CACHE STRING "CPU identifier for toolchains options")
# Set pre-requsites for selecting the STM32 microcontroller family type
set(STM32_TYPE "h7" CACHE STRING "STM32 HAL Microcontroller type")
# Set pre-requisite for exact device name of the STM32
# Set branch names for the sub modules dependencies
set(GITHUB_BRANCH_TOOLCHAIN "HEAD" CACHE STRING "git SHA for CMake Toolchain")

include(${cmake-toolchains_SOURCE_DIR}/frameworks/lib_cmsis.cmake)
