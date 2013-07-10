PROJECT(xbmcplatform)

CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
ENABLE_LANGUAGE(CXX)

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

SET(SOURCES src/util/XMLUtils.cpp)

ADD_LIBRARY(xbmcplatform ${SOURCES} ${PLAT_SOURCES})
TARGET_LINK_LIBRARIES(xbmcplatform ${CMAKE_THREAD_LIBS_INIT} ${TINYXML_LIBRARIES})
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
