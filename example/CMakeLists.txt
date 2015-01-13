# The name of our project is "HELLO". CMakeLists files in this project can 
# refer to the root source directory of the project as ${HELLO_SOURCE_DIR} and 
# to the root binary directory of the project as ${HELLO_BINARY_DIR}. 
cmake_minimum_required (VERSION 2.6) 
project (nanovg-example) 

include_directories (${PROJECT_SOURCE_DIR}/../src) 

add_executable (nanovg-example 
	${PROJECT_SOURCE_DIR}/example_gl3.c
	${PROJECT_SOURCE_DIR}/demo.c
	${PROJECT_SOURCE_DIR}/perf.c
	${PROJECT_SOURCE_DIR}/../src/nanovg.c
)

find_package(PkgConfig REQUIRED)
pkg_search_module(GLFW REQUIRED glfw3)
include_directories(${GLFW_INCLUDE_DIRS})
target_link_libraries(nanovg-example /usr/local/lib/lib${GLFW_STATIC_LIBRARIES}.dylib)

if(GLFW_FOUND)
    message(STATUS "Found GLFW ${GLFW_LIBRARIES}")
endif(GLFW_FOUND)


IF(APPLE)
   INCLUDE_DIRECTORIES ( /System/Library/Frameworks )
   FIND_LIBRARY(COCOA_LIBRARY Cocoa)
   FIND_LIBRARY(OpenGL_LIBRARY OpenGL )
   FIND_LIBRARY(CoreVideo_LIBRARY CoreVideo )
   FIND_LIBRARY(IOKit_LIBRARY IOKit )
   MARK_AS_ADVANCED (COCOA_LIBRARY
                     OpenGL_LIBRARY
                     CoreVideo_LIBRARY
                     IOKit_LIBRARY)
   SET(EXTRA_LIBS ${COCOA_LIBRARY} ${GLUT_LIBRARY} ${OpenGL_LIBRARY} ${CoreVideo_LIBRARY} ${IOKit_LIBRARY})

ENDIF (APPLE)

target_link_libraries(nanovg-example ${EXTRA_LIBS})






FIND_PACKAGE(OpenGL REQUIRED) 
MESSAGE("${OPENGL_INCLUDE_DIR}")

target_link_libraries(nanovg-example ${OPENGL_INCLUDE_DIR}) 

IF (WIN32)
FIND_PATH( GLEW_INCLUDE_PATH GL/glew.h
$ENV{PROGRAMFILES}/GLEW/include
${GLEW_ROOT_DIR}/include
DOC "The directory where GL/glew.h resides")

IF (NV_SYSTEM_PROCESSOR STREQUAL "AMD64")
FIND_LIBRARY( GLEW_LIBRARY
NAMES glew64 glew64s
PATHS
$ENV{PROGRAMFILES}/GLEW/lib
${PROJECT_SOURCE_DIR}/src/nvgl/glew/bin
${PROJECT_SOURCE_DIR}/src/nvgl/glew/lib
DOC "The GLEW library (64-bit)"
)
ELSE(NV_SYSTEM_PROCESSOR STREQUAL "AMD64")
FIND_LIBRARY( GLEW_LIBRARY
NAMES glew GLEW glew32 glew32s
PATHS
$ENV{PROGRAMFILES}/GLEW/lib
${PROJECT_SOURCE_DIR}/src/nvgl/glew/bin
${PROJECT_SOURCE_DIR}/src/nvgl/glew/lib
DOC "The GLEW library"
)
ENDIF(NV_SYSTEM_PROCESSOR STREQUAL "AMD64")
ELSE (WIN32)
FIND_PATH( GLEW_INCLUDE_PATH GL/glew.h
/usr/include
/usr/local/include
/sw/include
/opt/local/include
${GLEW_ROOT_DIR}/include
DOC "The directory where GL/glew.h resides")

FIND_LIBRARY( GLEW_LIBRARY
NAMES GLEW glew
PATHS
/usr/lib64
/usr/lib
/usr/local/lib64
/usr/local/lib
/sw/lib
/opt/local/lib
${GLEW_ROOT_DIR}/lib
DOC "The GLEW library")
ENDIF (WIN32)

SET(GLEW_FOUND "NO")
IF (GLEW_INCLUDE_PATH AND GLEW_LIBRARY)
SET(GLEW_LIBRARIES ${GLEW_LIBRARY})
SET(GLEW_FOUND "YES")
ENDIF (GLEW_INCLUDE_PATH AND GLEW_LIBRARY)


include(FindPackageHandleStandardArgs)
find_package_handle_standard_args(GLEW DEFAULT_MSG GLEW_LIBRARY GLEW_INCLUDE_PATH)
