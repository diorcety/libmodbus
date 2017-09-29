CMAKE_MINIMUM_REQUIRED(VERSION 2.6.0)

PROJECT(LIBMODBUS C)

SET(LIBMODBUS_VERSION_MAJOR 3)
SET(LIBMODBUS_VERSION_MINOR 1)
SET(LIBMODBUS_VERSION_MICRO 4)
SET(LIBMODBUS_VERSION ${LIBMODBUS_VERSION_MAJOR}.${LIBMODBUS_VERSION_MINOR}.${LIBMODBUS_VERSION_MICRO})

INCLUDE(CheckIncludeFiles)
INCLUDE(GNUInstallDirs)

SET(LIBRARY_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR})
SET(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR})

INCLUDE_DIRECTORIES(${CMAKE_CURRENT_BINARY_DIR}/include)

CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/config.h.in ${CMAKE_CURRENT_BINARY_DIR}/include/config.h)
CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/src/modbus-version.h.in ${CMAKE_CURRENT_BINARY_DIR}/include/modbus-version.h)

SET(HEADER_FILES
    src/modbus.h
    src/modbus-private.h
    src/modbus-rtu.h
    src/modbus-rtu-private.h
    src/modbus-tcp.h
    src/modbus-tcp-private.h
    src/modbus-version.h.in)

SET(SOURCE_FILES
    src/modbus.c
    src/modbus-tcp.c
    src/modbus-rtu.c
    src/modbus-data.c)

ADD_LIBRARY(modbus ${HEADER_FILES} ${SOURCE_FILES})
IF(WIN32)
  TARGET_LINK_LIBRARIES(modbus wsock32 ws2_32)
endif(WIN32)

#------------------------------------------------------------------------------

INSTALL(FILES src/modbus.h DESTINATION include/modbus)
INSTALL(FILES src/modbus-rtu.h DESTINATION include/modbus)
INSTALL(FILES src/modbus-tcp.h DESTINATION include/modbus)
INSTALL(FILES ${CMAKE_CURRENT_BINARY_DIR}/include/modbus-version.h DESTINATION include/modbus)

INSTALL(TARGETS modbus
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})
