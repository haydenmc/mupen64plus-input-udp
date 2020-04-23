cmake_minimum_required(VERSION 2.8.11)
project(mupen64plus-input-udp)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)
set(M64_APIDIR "${CMAKE_SOURCE_DIR}/../../../mupen64plus-core/src/api")

message("${CMAKE_SOURCE_DIR}/../../src/")

# State directories for modules and binaries
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/CMake)
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/CMake/Find)

set(CMAKE_CC_FLAGS "-ffast-math -fno-strict-aliasing -fvisibility=hidden -D_GNU_SOURCE=1")

set(CMAKE_CXX_FLAGS "-pthread -fvisibility-inlines-hidden")

set(SRCS
  ${CMAKE_SOURCE_DIR}/../../src/plugin.c
  )

if(WIN32)
  set(SRCS
    ${SRCS}
    ${CMAKE_SOURCE_DIR}/../../src/osal_dynamiclib_win32.c
    )
else()
  set(SRCS
    ${SRCS}
    ${CMAKE_SOURCE_DIR}/../../src/osal_dynamiclib_unix.c
    )
endif()


# Specify include directories
include_directories(
  ${M64_APIDIR}
  ${CMAKE_SOURCE_DIR}/../../src/
  )
# Create the binary
add_library(${CMAKE_PROJECT_NAME} SHARED ${SRCS})

# Link the libraries
# add_dependencies(${CMAKE_PROJECT_NAME} ${SDL2_LIBRARIES})

if(WIN32)
  target_link_libraries(${CMAKE_PROJECT_NAME}
    )
elseif(APPLE)
  target_link_libraries(${CMAKE_PROJECT_NAME}
    )
else()
  target_link_libraries(${CMAKE_PROJECT_NAME}
    dl
    )
endif()
