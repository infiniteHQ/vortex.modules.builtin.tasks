cmake_minimum_required(VERSION 3.0)
project(module)

# Inclure la bibliothèque partagée vortex_shared
find_library(VORTEX_SHARED_LIBRARY vortex_shared HINTS /lib)

# Vérifier si la bibliothèque a été trouvée
if(NOT VORTEX_SHARED_LIBRARY)
    message(FATAL_ERROR "Please build/install the vortex core before compiling modules !")
endif()


file(GLOB_RECURSE SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp")
add_library(module SHARED ${SOURCES})

# Compile & link
target_compile_options(module PRIVATE -Wall -Wextra)
target_link_libraries(module PRIVATE ${VORTEX_SHARED_LIBRARY})
target_include_directories(module PRIVATE /usr/include/Vortex)

set(MODULE_TAG "vortex.modules.builtin.tasks.1.1.0")
set(INSTALL_INCLUDE_DIR /usr/local/include/Vortex/modules/${MODULE_TAG}/)

# Installation des fichiers d'en-tête de /main/include
install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/
        DESTINATION ${INSTALL_INCLUDE_DIR}/
        PATTERN "*.h"
        PATTERN "*.hpp"
        PATTERN "*.inl"
)