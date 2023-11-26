cmake_minimum_required(VERSION 3.27)
project(nosejas)

set(CMAKE_VERBOSE_MAKEFILE ON)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_EXTENSIONS OFF)

if(true)
  set(THIRD_PARTY "${CMAKE_SOURCE_DIR}/third_party")
else()
  set(THIRD_PARTY "C:/msdk/botsunited")
endif()

set(SRC_DIR "${CMAKE_SOURCE_DIR}/src")
set(AMXXSDK "${THIRD_PARTY}/amxmodx/public")
set(METAMOD "${THIRD_PARTY}/metamod-hl1/metamod")
set(HLSDK "${THIRD_PARTY}/hlsdk")

set(API_INCLUDE_PATH "${HLSDK}/common ${HLSDK}/dlls ${HLSDK}/engine ${HLSDK}/pm_shared ${HLSDK}/game_shared ${HLSDK}/public ${METAMOD}")
string(REPLACE " " ";" API_INCLUDE_PATH_LIST ${API_INCLUDE_PATH})

include_directories(${API_INCLUDE_PATH_LIST})
include_directories(${AMXXSDK})
include_directories(${AMXXSDK}/sdk)
include_directories(${SRC_DIR}/helper)

add_definitions(-DHAVE_STDINT_H)
add_definitions(-DUSE_METAMOD)
file(GLOB_RECURSE SOURCE_FILES ${SRC_DIR}/*.h ${SRC_DIR}/*.cpp ${AMXXSDK}/sdk/*.cpp)

add_library(${PROJECT_NAME} SHARED ${SOURCE_FILES})

set(FLAGS "-m32 -O3 -mtune=generic -fvisibility=hidden -flto -fPIC")
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_FLAGS "${FLAGS} -fdata-sections -ffunction-sections -fpermissive")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wzero-as-null-pointer-constant -Wunknown-pragmas")

set_target_properties(${PROJECT_NAME} PROPERTIES PREFIX "")
set_target_properties(${PROJECT_NAME} PROPERTIES SUFFIX "_amxx.dll")

set(LIBRARY_OUTPUT_PATH "C:/steamcmd/cs_server/cstrike/addons/amxmodx/modules")

# C:/steamcmd/cs_server/start_server_dev.lnk
