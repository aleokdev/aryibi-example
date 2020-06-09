cmake_minimum_required(VERSION 3.15)
project(aryibi-example-proj LANGUAGES CXX C)

set(CMAKE_CXX_STANDARD 17)

add_executable(aryibi-example src/main.cpp)

add_subdirectory(lib)

file(GLOB ARYIBI_EXAMPLE_ASSETS assets/* lib/aryibi/assets/*.frag lib/aryibi/assets/*.vert)
set(ARYIBI_EXAMPLE_BINARY_ASSETS "")
foreach(ASSET ${ARYIBI_EXAMPLE_ASSETS})
    file(RELATIVE_PATH ASSET_RELATIVE_PATH ${ASSET}/../.. ${ASSET})
    message(STATUS "Found aryibi example asset: ${ASSET_RELATIVE_PATH}")
    set(ASSET_IN_BINARY_PATH ${CMAKE_CURRENT_BINARY_DIR}/${ASSET_RELATIVE_PATH})
    set(ARYIBI_EXAMPLE_BINARY_ASSETS ${ARYIBI_EXAMPLE_BINARY_ASSETS} ${ASSET_IN_BINARY_PATH})

    add_custom_command(OUTPUT ${ASSET_IN_BINARY_PATH}
            DEPENDS ${ASSET}
            COMMAND cmake -E copy ${ASSET} ${ASSET_IN_BINARY_PATH}
            COMMENT "Copying ${ASSET_RELATIVE_PATH} to binary directory." VERBATIM)
endforeach()

add_custom_target(copy-aryibi-example-assets DEPENDS ${ARYIBI_EXAMPLE_BINARY_ASSETS})
add_dependencies(aryibi-example copy-aryibi-example-assets)