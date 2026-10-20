# SPDX-License-Identifier: GPL-2.0-only
#
# Build-only superbuild: builds every i1n1 module repository in dependency order.
cmake_minimum_required(VERSION 3.22)

# Same deployment target as the other i1n1 repositories so C objects agree.
if(CMAKE_HOST_APPLE AND NOT CMAKE_OSX_DEPLOYMENT_TARGET)
  set(CMAKE_OSX_DEPLOYMENT_TARGET "11.0" CACHE STRING "Minimum macOS deployment target")
endif()

project(i1n1 VERSION 0.1.0 LANGUAGES C)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set_property(GLOBAL PROPERTY I1N1_REPO_UMBRELLA_LOADED TRUE)
include("${CMAKE_CURRENT_LIST_DIR}/cmake/i1n1_deps.cmake")

# Order matters: core defines the shared warning policy and helper functions,
# engine and platform come before the repositories that link against them.
i1n1_require(core)
i1n1_require(engine)
i1n1_require(platform)
i1n1_require(driverzone)
i1n1_require(host_efi)
