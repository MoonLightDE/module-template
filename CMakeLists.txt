#    Copyright (C) 2014 Moonlight Desktop Environment Team
#    Authors:
#       Alexis López Zubieta
#
#    This file is part of the Moonlight Desktop Environment 
#
#    Moonlight Desktop Environment is free software you can redistribute it 
#    and/or modify it under the terms of the GNU General Public License as 
#    published by the Free Software Foundation, either version 3 of the License,
#    or (at your option) any later version.
#
#    Moonlight Desktop Environment is distributed in the hope that it will be
#    useful, but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU General Public License for more details.
#
#    You should have received a copy of the GNU General Public License
#    along with Moonlight Desktop Environment.  If not, see 
#    <http://www.gnu.org/licenses/>.
#

project(moonlight-module-template)
set(${PROJECT_NAME}_MAJOR_VERSION 0)
set(${PROJECT_NAME}_MINOR_VERSION 6)
set(${PROJECT_NAME}_PATCH_VERSION 0)

set(${PROJECT_NAME}_VERSION ${${PROJECT_NAME}_MAJOR_VERSION}.${${PROJECT_NAME}_MINOR_VERSION}.${${PROJECT_NAME}_PATCH_VERSION})

# ------------------------------------------------------------------------------
# Configuration
# ------------------------------------------------------------------------------
cmake_minimum_required(VERSION 2.8)

## Build Type
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    message(STATUS "Setting build type to 'Debug' as none was specified.")
    set(CMAKE_BUILD_TYPE Debug CACHE STRING "Choose the type of build." FORCE)
    
    set(CMAKE_CXX_FLAGS_DEBUG "-g3 -gdwarf-2")
    message(STATUS "Using CXX_FLAGS: ${CMAKE_CXX_FLAGS_DEBUG}")

    # Set the possible values of build type for cmake-gui
    set_property(CACHE CMAKE_BUILD_TYPE PROPERTY
        STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
    set( STANDALONE_BUILD 1)
    message(STATUS "Core standalone build")
else(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
    set( STANDALONE_BUILD 0)
    message(STATUS "Core buld inside ${CMAKE_PROJECT_NAME}")
endif(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)

# ------------------------------------------------------------------------------
# Build
# ------------------------------------------------------------------------------
add_subdirectory(src)


# ------------------------------------------------------------------------------
# Distribution
# ------------------------------------------------------------------------------
if(STANDALONE_BUILD EQUAL 1)
    INCLUDE(InstallRequiredSystemLibraries)

    SET(CPACK_PACKAGE_DESCRIPTION_SUMMARY "MoonLight Module Template")
    SET(CPACK_PACKAGE_VENDOR "")
    SET(CPACK_PACKAGE_DESCRIPTION_FILE "${CMAKE_CURRENT_SOURCE_DIR}/README.md")
    SET(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/LICENSE")
    SET(CPACK_PACKAGE_VERSION_MAJOR ${${PROJECT_NAME}_MAJOR_VERSION} )
    SET(CPACK_PACKAGE_VERSION_MINOR ${${PROJECT_NAME}_MINOR_VERSION} )
    SET(CPACK_PACKAGE_VERSION_PATCH ${${PROJECT_NAME}_PATCH_VERSION} )
    SET(CPACK_PACKAGE_INSTALL_DIRECTORY "CMake ${CMake_VERSION_MAJOR}.${CMake_VERSION_MINOR}")

    SET(CPACK_SET_DESTDIR "ON")
    ## Debian especific packing instructions
    SET(CPACK_PACKAGE_CONTACT "MoonLightDE Mail List <moonlightde@googlegroups.com>")
    SET(CPACK_DEBIAN_PACKAGE_DEPENDS "moonlight-core")

    INCLUDE(CPack)
endif(STANDALONE_BUILD EQUAL 1)
