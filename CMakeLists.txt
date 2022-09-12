cmake_minimum_required(VERSION 3.3)

set(PROJECT flight-processor)
set(CMAKE_SOURCE_DIR src)

project(${PROJECT})

set(SRC
    src/main.c)

# set(HDR
#     .h)

set(SOURCE_FILES ${SRC} ${HDR})
list(SORT SOURCE_FILES)

add_executable(${PROJECT} ${SOURCE_FILES})

link_libraries(m)
if (APPLE)
    find_library(XPLM_LIBRARY XPLM ./lib/xplane-sdk/Libraries/Mac)

    target_link_libraries(${PROJECT}
        ${XPLM_LIBRARY}
    )
endif()

include_directories(${PROJECT} PUBLIC "./lib/xplane-sdk/CHeaders/XPLM/"
    "./lib/xplane-sdk/CHeaders/Widgets" "../OpenAL/include" "./lib/xplane-sdk")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DXPLM200=1 -DXPLM210=1")

if(APPLE)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DAPL=1 -DIBM=0 -DLIN=0")
else()
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DAPL=0 -DIBM=0 -DLIN=1")
endif()

# compiler flags
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra -Werror --std=c99 \
    -Wno-unused-local-typedefs -target x86_64-apple-darwin")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} \
    -DCHECK_RESULT_USED=\"__attribute__ ((warn_unused_result))\"")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D_GNU_SOURCE -D_FILE_OFFSET_BITS=64")
