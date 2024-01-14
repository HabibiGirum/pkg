# this is my CMakeLists.txt file and add installation type on this 
# CMakeLists.txt
cmake_minimum_required(VERSION 3.10)
project(osquery)

# Install the script file to /usr/local/bin
# install(PROGRAMS script.sh DESTINATION /usr/local/bin)

# Install the executable file to /usr/local/bin
# install(PROGRAMS mytool DESTINATION /usr/local/bin)

# Install the library file to /usr/local/lib

# install(FILES lib/libfoo.so DESTINATION /usr/local/lib)
message("CMAKE_CURRENT_SOURCE_DIR: ${CMAKE_CURRENT_SOURCE_DIR}")
message("CPACK_DMG_BACKGROUND_IMAGE: ${CPACK_DMG_BACKGROUND_IMAGE}")

set(OSQUERY_DATA_PATH "${CMAKE_CURRENT_SOURCE_DIR}/windowspackage")

set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/assets/License.txt")
set(CPACK_RESOURCE_FILE_BACKGROUND_IMAGE "${CMAKE_CURRENT_SOURCE_DIR}/assets/Background.PNG")
set(CPACK_RESOURCE_FILE_README "${CMAKE_CURRENT_SOURCE_DIR}/assets/Readme.txt")
set(CPACK_RESOURCE_FILE_WELCOME "${CMAKE_CURRENT_SOURCE_DIR}/assets/welcome.txt")

# set(CPACK_POST_INSTALL_SCRIPT "${CMAKE_CURRENT_SOURCE_DIR}/postinstall.sh")


# Set the package name and version
set(CPACK_PACKAGE_VERSION "1.0.0")
set(VISTAR_PACKAGE_RELEASE "1.macos")
set(CPACK_PACKAGE_NAME "Vistar")
# set(CPACK_PACKAGE_FILE_NAME "${CMAKE_PROJECT_NAME}-${CMAKE_PROJECT_VERSION}-${VISTAR_PACKAGE_RELEASE}")

# Set the generator to productbuild
# set(CPACK_GENERATOR "productbuild")
set(CPACK_PACKAGE_RELOCATABLE ON)
set(CPACK_RESOURCE_FILE_LICENSE "${OSQUERY_DATA_PATH}/control/LICENSE.txt")

include("platform/windows/common.cmake")
include("platform/windows/wix.cmake")

# Include CPack module
include(CPack)