# cmake requirements
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

# project id
PROJECT(OmnidomeServer)

# force outut into several folders
SET (CMAKE_LIBRARY_OUTPUT_DIRECTORY
  ${PROJECT_BINARY_DIR}/bin )
SET (CMAKE_RUNTIME_OUTPUT_DIRECTORY
  ${PROJECT_BINARY_DIR}/bin )
SET (CMAKE_ARCHIVE_OUTPUT_DIRECTORY
  ${PROJECT_BINARY_DIR}/bin )
SET (CMAKE_CURRENT_BINARY_DIR
  ${PROJECT_BINARY_DIR}/moc )
SET (CMAKE_PROJECT_DOC_DIR
  ${PROJECT_BINARY_DIR}/doc )

SET(omnidome_server_src src/main.c) 

ADD_DEFINITIONS("-DDEBUG")
INCLUDE_DIRECTORIES(./include /usr/include/gstreamer-1.0 /usr/include/glib-2.0 /usr/lib/x86_64-linux-gnu/glib-2.0/include   )

SET(OpenGL_LIBRARY GL GLU GLEW )
SET(EXTRA_LIBS ${EXTRA_LIBS} ${OpenGL_LIBRARY}) 

ADD_EXECUTABLE(omnidome-server ${omnidome_server_src} )

TARGET_LINK_LIBRARIES(omnidome-server -lgstreamer-1.0 -lgobject-2.0 -lglib-2.0 )

IF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
  ADD_DEFINITIONS(-g -Os -fsigned-char -Wall -fPIC -Wno-deprecated-register)
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")

#ADD_DEFINITIONS(-ferror-limit=5 -fcolor-diagnostics -fdiagnostics-show-template-tree)

# add a target to generate API documentation with Doxygen
FIND_PACKAGE(Doxygen)
IF(DOXYGEN_FOUND)
  CONFIGURE_FILE(${CMAKE_PROJECT_DOC_DIR}/omnidome-server.doxygen ${CMAKE_PROJECT_DOC_DIR} @ONLY)
  ADD_CUSTOM_TARGET(doc
    ${DOXYGEN_EXECUTABLE} ${CMAKE_PROJECT_DOC_DIR}/omnidome-server.doxygen
    WORKING_DIRECTORY ${PROJECT_BINARY_DIR}
    COMMENT "Generating API documentation with Doxygen (see ./doc/html/index.html)" VERBATIM
    )
ENDIF(DOXYGEN_FOUND)
