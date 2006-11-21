CMAKE_MINIMUM_REQUIRED(VERSION 2.0)
  
PROJECT(VMTK)

SUBDIRS(
  PypeS
  vtkVmtk
  vmtkScripts
  )

OPTION(VMTK_MINIMAL_INSTALL "Do not install individual scripts, only vmtk.py" OFF)

SET(LIBRARY_OUTPUT_PATH ${VMTK_BINARY_DIR}/bin CACHE PATH
    "Single output directory for building all libraries.")
SET(EXECUTABLE_OUTPUT_PATH ${VMTK_BINARY_DIR}/bin CACHE PATH
    "Single output directory for building all executables.")
MARK_AS_ADVANCED(LIBRARY_OUTPUT_PATH EXECUTABLE_OUTPUT_PATH)

SET(VMTK_SRCS
   vmtk.py
   )

IF(NOT VMTK_INSTALL_BIN_DIR)
  SET(VMTK_INSTALL_BIN_DIR ${VMTK_INSTALL_ROOT}/bin)
ENDIF(NOT VMTK_INSTALL_BIN_DIR)

FOREACH (SCRIPT_FILE  ${VMTK_SRCS})
  CONFIGURE_FILE(${VMTK_SOURCE_DIR}/${SCRIPT_FILE} ${VMTK_BINARY_DIR}/${SCRIPT_FILE} COPYONLY)
ENDFOREACH (SCRIPT_FILE)

IF (NOT WIN32)

SET (STRIPPED_SCRIPTS_SRCS )
FOREACH (SCRIPT_FILE  ${VMTK_SRCS})
  CONFIGURE_FILE(${VMTK_SOURCE_DIR}/${SCRIPT_FILE} ${VMTK_BINARY_DIR}/${SCRIPT_FILE} COPYONLY)
  STRING(REGEX REPLACE ".py" "" STRIPPED_SCRIPT_FILE ${SCRIPT_FILE})
  CONFIGURE_FILE(${VMTK_SOURCE_DIR}/${SCRIPT_FILE} ${VMTK_BINARY_DIR}/${STRIPPED_SCRIPT_FILE} COPYONLY)
  SET (STRIPPED_SCRIPTS_SRCS ${STRIPPED_SCRIPTS_SRCS} ${STRIPPED_SCRIPT_FILE})
ENDFOREACH (SCRIPT_FILE)

INSTALL_PROGRAMS(${VMTK_INSTALL_BIN_DIR} FILES ${STRIPPED_SCRIPTS_SRCS})

ELSE (NOT WIN32)

INSTALL_PROGRAMS(${VMTK_INSTALL_BIN_DIR} FILES ${VMTK_SRCS})

ENDIF (NOT WIN32)