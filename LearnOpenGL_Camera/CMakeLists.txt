cmake_minimum_required(VERSION 3.10.2)
set(CMAKE_CXX_STANDARD_REQUIRED 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
project(LEARNOPENGL_CAMERA VERSION 0.1.0)
message(STATUS "PROJECT_NAME: ${PROJECT_NAME}")

file(GLOB SOURCES "*.cpp")

add_executable(${PROJECT_NAME} ${SOURCES})
target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_17)

set(CMAKE_CONFIGURATION_TYPES "Debug;Release;MinSizeRel;RelWithDebInfo" CACHE STRING "" FORCE)

#############################
# Compiler and Linker Flags #
#############################
message(STATUS "using ${CMAKE_GENERATOR}")
if (CMAKE_GENERATOR STREQUAL "MinGW Makefiles")
    if (NOT MINGW_PATH) 
      message(FATAL_ERROR "MINGW_PATH not set in .env.cmake")
    endif()
    set(USE_MINGW "True")
    set(CMAKE_C_COMPILER ${MINGW_PATH}/bin/gcc.exe)
    set(CMAKE_CXX_COMPILER  ${MINGW_PATH}/bin/g++.exe)
endif()
if (CMAKE_GENERATOR STREQUAL "MinGW Makefiles")
    message(STATUS "CMAKE_GENERATOR: MinGW Makefiles")
else()
    message(STATUS "CMAKE_GENERATOR: ${CMAKE_GENERATOR}")
endif()

# Add libraries
# Add OpenGL
find_package(OpenGL REQUIRED)
if (OPENGL_FOUND)
    message(STATUS "OPENGL_INCLUDE_DIRS: ${OPENGL_INCLUDE_DIRS}")
    message(STATUS "OPENGL_LIBRARIES: ${OPENGL_LIBRARIES}")
    message(STATUS "OPENGL_VERSION: ${OPENGL_VERSION}")
    include_directories(${OPENGL_INCLUDE_DIRS})
    target_link_libraries(${PROJECT_NAME} ${OPENGL_LIBRARIES})
else()
    message(FATAL_ERROR "OpenGL not found")
endif()
# Add Glew using find package
find_package(GLEW REQUIRED)
if (GLEW_FOUND)
    message(STATUS "GLEW_INCLUDE_DIRS: ${GLEW_INCLUDE_DIRS}")
    message(STATUS "GLEW_LIBRARIES: ${GLEW_LIBRARIES}")
    message(STATUS "GLEW_VERSION: ${GLEW_VERSION}")
    include_directories(${GLEW_INCLUDE_DIRS})
    target_link_libraries(${PROJECT_NAME} ${GLEW_LIBRARIES})
else()
    message(FATAL_ERROR "GLEW not found")
endif()
# Add the stb library using find package
include_directories(/usr/include/stb/)
# Add the glm library using find package
find_package(glm REQUIRED)
if (glm_FOUND)
    message(STATUS "glm_INCLUDE_DIRS: ${glm_INCLUDE_DIRS}")
    message(STATUS "glm_LIBRARIES: ${glm_LIBRARIES}")
    message(STATUS "glm_VERSION: ${glm_VERSION}")
    include_directories(${glm_INCLUDE_DIRS})
    target_link_libraries(${PROJECT_NAME} ${glm_LIBRARIES})
else()
    message(FATAL_ERROR "glm not found")
endif()
# Add the glfw library using find package
find_package(glfw3 REQUIRED)
message("GLFW found!")
message("GLFW version: ${glfw3_VERSION}")

if (glfw3_FOUND)



#    message(STATUS "glfw3_INCLUDE_DIRS: ${glfw_INCLUDE_DIRS}")
#    message(STATUS "glfw3_LIBRARIES: ${GLFW3_LIBRARY}")
    message(STATUS "glfw3_VERSION: ${glfw3_VERSION}")
    include_directories(${glfw3_INCLUDE_DIRS})
    target_link_libraries(${PROJECT_NAME} ${glfw3_LIBRARIES})
    target_link_libraries(${PROJECT_NAME} glfw)
else()
    message(FATAL_ERROR "glfw3 not found")
endif()
