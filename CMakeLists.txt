PROJECT(xbmcplatform)

CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
ENABLE_LANGUAGE(CXX)

SET(xbmcplatform_NAME xbmcplatform)
SET(xbmcplatform_DESCRIPTION "XBMC addon-on platform support library")
SET(xbmcplatform_VERSION_MAJOR 13)
SET(xbmcplatform_VERSION_MINOR 0)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${PROJECT_SOURCE_DIR})

FIND_PACKAGE(TinyXML REQUIRED)
INCLUDE(UseMultiArch.cmake)

FIND_PACKAGE(Threads REQUIRED)

IF(WIN32)
  SET(PLAT_SOURCES src/windows/dlfcn-win32.cpp
                   src/windows/os-threads.cpp
                   src/windows/serialport.cpp)
ELSE(WIN32)
  SET(PLAT_SOURCES src/posix/serialport.cpp)
ENDIF(WIN32)

SET(xbmcplatform_INCLUDE_DIRS ${TINYXML_INCLUDE_DIR})
SET(xbmcplatform_LIBRARIES ${CMAKE_THREAD_LIBS_INIT} ${TINYXML_LIBRARIES})
SET(xbmcplatform_LIBRARY "-lxbmcplatform")

SET(SOURCES src/util/XMLUtils.cpp)

ADD_LIBRARY(xbmcplatform ${SOURCES} ${PLAT_SOURCES})
TARGET_LINK_LIBRARIES(xbmcplatform ${DEPLIBS})
SET_TARGET_PROPERTIES(xbmcplatform PROPERTIES VERSION 13.0.0
                                              SOVERSION 13.0)

INSTALL(TARGETS xbmcplatform DESTINATION ${CMAKE_INSTALL_LIBDIR})
INSTALL(FILES src/os.h DESTINATION include/xbmc)
INSTALL(FILES src/posix/os-socket.h
              src/posix/os-threads.h
              src/posix/os-types.h
        DESTINATION include/xbmc/posix)
INSTALL(FILES src/sockets/serialport.h
              src/sockets/socket.h
              src/sockets/tcp.h
        DESTINATION include/xbmc/sockets)
INSTALL(FILES src/threads/mutex.h
              src/threads/threads.h
        DESTINATION include/xbmc/threads)
INSTALL(FILES src/util/atomic.h
              src/util/baudrate.h
              src/util/buffer.h
              src/util/StdString.h
              src/util/timeutils.h
              src/util/util.h
              src/util/XMLUtils.h
        DESTINATION include/xbmc/util)

# Pkgconfig
INCLUDE(PkgConfigHandler.cmake)
configure_pc_file(xbmcplatform xbmcplatform.pc.in
                               xbmcplatform.pc
                               ${CMAKE_INSTALL_PREFIX}
                               ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}
                               ${CMAKE_INSTALL_PREFIX}/include)

INSTALL(FILES ${CMAKE_BINARY_DIR}/xbmcplatform.pc DESTINATION lib/pkgconfig)

# config mode
configure_file (xbmcplatform-config.cmake.in
                xbmcplatform-config.cmake @ONLY)
INSTALL(FILES ${CMAKE_BINARY_DIR}/xbmcplatform-config.cmake DESTINATION lib/xbmcplatform)
