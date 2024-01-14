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

set(OSQUERY_DATA_PATH "${CMAKE_CURRENT_SOURCE_DIR}/macpack")

set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/assets/License.txt")
set(CPACK_RESOURCE_FILE_BACKGROUND_IMAGE "${CMAKE_CURRENT_SOURCE_DIR}/assets/Background.PNG")
set(CPACK_RESOURCE_FILE_README "${CMAKE_CURRENT_SOURCE_DIR}/assets/Readme.txt")
set(CPACK_RESOURCE_FILE_WELCOME "${CMAKE_CURRENT_SOURCE_DIR}/assets/welcome.txt")

set(CPACK_POST_INSTALL_SCRIPT "${CMAKE_CURRENT_SOURCE_DIR}/postinstall.sh")


# Set the package name and version
set(CPACK_PACKAGE_VERSION "1.0.0")
set(VISTAR_PACKAGE_RELEASE "1.macos")
set(CPACK_PACKAGE_NAME "Vistar")
# set(CPACK_PACKAGE_FILE_NAME "${CMAKE_PROJECT_NAME}-${CMAKE_PROJECT_VERSION}-${VISTAR_PACKAGE_RELEASE}")

# Set the generator to productbuild
set(CPACK_GENERATOR "productbuild")
set(CPACK_PACKAGE_RELOCATABLE ON)
set(CPACK_RESOURCE_FILE_LICENSE "${OSQUERY_DATA_PATH}/control/LICENSE.txt")

# include("platform/macos/common.cmake")
##################
#
# Copyright (c) 2014-present, The osquery authors
#
# This source code is licensed as defined by the LICENSE file found in the
# root directory of this source tree.
#
# SPDX-License-Identifier: (Apache-2.0 OR GPL-2.0-only)
#

set(VISTAR_PACKAGE_RELEASE "1.macos")
set(CPACK_PACKAGE_FILE_NAME "${CPACK_PACKAGE_NAME}-${CPACK_PACKAGE_VERSION}_${VISTAR_PACKAGE_RELEASE}")
set(CPACK_SET_DESTDIR ON)

install(
  DIRECTORY
    "${OSQUERY_DATA_PATH}/opt/osquery/osquery.app"

  DESTINATION
    "/opt/osquery/lib"

  USE_SOURCE_PERMISSIONS

  COMPONENT
    osquery
)

execute_process(
  COMMAND "${CMAKE_COMMAND}" -E create_symlink "/opt/osquery/lib/osquery.app/Contents/MacOS/osqueryd" osqueryi
  WORKING_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}"
)

execute_process(
  COMMAND "${CMAKE_COMMAND}" -E create_symlink "/opt/osquery/lib/osquery.app/Contents/Resources/osqueryctl" osqueryctl
  WORKING_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}"
)

install(
  FILES
    "${CMAKE_CURRENT_BINARY_DIR}/osqueryi"
    "${CMAKE_CURRENT_BINARY_DIR}/osqueryctl"
  
  DESTINATION
    "/usr/local/bin/"
  
  COMPONENT
    osquery
)

install(
  DIRECTORY "${OSQUERY_DATA_PATH}/private/var/osquery"
  DESTINATION "/private/var"
  COMPONENT osquery
)

install(
  DIRECTORY
  DESTINATION "/private/var/log/osquery"
  COMPONENT osquery
)

##################
# include("platform/macos/productbuild.cmake")

##########################
#
# Copyright (c) 2014-present, The osquery authors
#
# This source code is licensed as defined by the LICENSE file found in the
# root directory of this source tree.
#
# SPDX-License-Identifier: (Apache-2.0 OR GPL-2.0-only)
#

set(CPACK_PACKAGE_FILE_NAME "${CPACK_PACKAGE_NAME}-${CPACK_PACKAGE_VERSION}")
set(CPACK_COMMAND_PRODUCTBUILD "${OSQUERY_DATA_PATH}/control/pkg/productbuild.sh")
# set(CPACK_COMMAND_PKGBUILD "${CPACK_COMMAND_PRODUCTBUILD}")

install(
  FILES
    "${OSQUERY_DATA_PATH}/control/pkg/io.osquery.agent.conf"
    "${OSQUERY_DATA_PATH}/control/pkg/io.osquery.agent.plist"

  DESTINATION
    "/private/var/osquery"

  COMPONENT
    osquery
)

#############################

# Include CPack module
include(CPack)