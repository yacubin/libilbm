cmake_minimum_required(VERSION 3.10)
project(libilbm VERSION 0.1.2 DESCRIPTION "EA-85 ILBM parser library" LANGUAGES C)

include(GNUInstallDirs)
include(CheckIncludeFile)

find_package(IFF REQUIRED)

set(VERSION "${PROJECT_VERSION}")
set(prefix "${CMAKE_INSTALL_PREFIX}")
set(exec_prefix "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_BINDIR}")
set(libdir "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}")
set(includedir "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_INCLUDEDIR}")

set(ilbm_BUILD_TYPE SHARED)
if(DEFINED BUILD_SHARED_LIBS AND NOT BUILD_SHARED_LIBS)
  set(ilbm_BUILD_TYPE STATIC)
endif ()

check_include_file(getopt.h HAVE_GETOPT_H)
if(HAVE_GETOPT_H)
  set(HAVE_GETOPT_H 1)
else ()
  set(HAVE_GETOPT_H 0)
endif ()

set(ilbm_HEADERS
  src/libilbm/bitmapheader.h
  src/libilbm/byterun.h
  src/libilbm/cmykmap.h
  src/libilbm/colormap.h
  src/libilbm/colornames.h
  src/libilbm/colorrange.h
  src/libilbm/cycleinfo.h
  src/libilbm/destmerge.h
  src/libilbm/dpiheader.h
  src/libilbm/drange.h
  src/libilbm/grab.h
  src/libilbm/ilbm.h
  src/libilbm/ilbmchunkheaders.h
  src/libilbm/ilbmextensions.h
  src/libilbm/ilbmformextensions.h
  src/libilbm/ilbmimage.h
  src/libilbm/interleave.h
  src/libilbm/sprite.h
  src/libilbm/viewport.h
  )

set(ilbm_SOURCES
  src/libilbm/bitmapheader.c
  src/libilbm/byterun.c
  src/libilbm/cmykmap.c
  src/libilbm/colormap.c
  src/libilbm/colornames.c
  src/libilbm/colorrange.c
  src/libilbm/cycleinfo.c
  src/libilbm/destmerge.c
  src/libilbm/dpiheader.c
  src/libilbm/drange.c
  src/libilbm/grab.c
  src/libilbm/ilbm.c
  src/libilbm/ilbmimage.c
  src/libilbm/interleave.c
  src/libilbm/sprite.c
  src/libilbm/viewport.c
  )

set(ilbm_DATAFILES
  src/libilbm/libilbm.def
  src/libilbm/libilbm.vcxproj
  src/libilbm/libilbm.vcxproj.filters
  src/libilbm/Makefile.am
  src/libilbm.pc.in
  src/libilbm.sln
  doc/ACBM.asc
  doc/additions
  doc/additions/ILBM.asc
  doc/additions/ILBM.DRNG.asc
  doc/additions/PBM.asc
  doc/additions/README.md
  doc/ILBM-appendix.asc
  doc/ILBM.asc
  doc/ILBM.DRNG.asc
  doc/ILBM_CMYK.txt
  doc/ILBM_CNAM.txt
  doc/ILBM_DPI.txt
  doc/Makefile.am
  AUTHORS
  bootstrap
  ChangeLog
  configure.ac
  COPYING
  Makefile.am
  NEWS
  README.md
  release.nix
  src/Makefile.am
  version
  )

set(ilbm_INCLUDES
  "${CMAKE_CURRENT_BINARY_DIR}"
  "${CMAKE_CURRENT_SOURCE_DIR}/src/libilbm"
  )

set(ilbm_LIBRARIES
  IFF::IFF
  )

set(ilbm_DEFINITIONS
  LIBILBM_EXPORTS
  )

if (WIN32)
  add_definitions(-DWIN32)
endif ()

configure_file(src/libilbm.pc.in "${CMAKE_CURRENT_BINARY_DIR}/libilbm.pc" @ONLY UNIX)

source_group(TREE "${CMAKE_CURRENT_SOURCE_DIR}" FILES ${ilbm_HEADERS} ${ilbm_SOURCES} ${ilbm_DATAFILES})
add_library(ilbm ${ilbm_BUILD_TYPE} ${ilbm_HEADERS} ${ilbm_SOURCES} ${ilbm_DATAFILES})
target_include_directories(ilbm PUBLIC "$<BUILD_INTERFACE:${ilbm_INCLUDES}>")
target_compile_definitions(ilbm PRIVATE "${ilbm_DEFINITIONS}")
target_link_libraries(ilbm PUBLIC ${ilbm_LIBRARIES})
set_target_properties(ilbm PROPERTIES VERSION ${PROJECT_VERSION} SOVERSION ${PROJECT_VERSION_MAJOR})
set_target_properties(ilbm PROPERTIES PREFIX "lib")
set_property(TARGET ilbm PROPERTY POSITION_INDEPENDENT_CODE ON)

add_library(ILBM::ILBM ALIAS ilbm)


set(acbm2ilbm_HEADERS
  src/acbm2ilbm/acbm2ilbm.h
  )

set(acbm2ilbm_SOURCES
  src/acbm2ilbm/acbm2ilbm.c
  src/acbm2ilbm/main.c
  )

set(acbm2ilbm_DATAFILES
  src/acbm2ilbm/acbm2ilbm.vcxproj
  src/acbm2ilbm/acbm2ilbm.vcxproj.filters
  src/acbm2ilbm/Makefile.am
  )

set(acbm2ilbm_DEFINITIONS
  PACKAGE_NAME=\"${PROJECT_NAME}\"
  PACKAGE_VERSION=\"${PROJECT_VERSION}\"
  HAVE_GETOPT_H=${HAVE_GETOPT_H}
  )

source_group(TREE "${CMAKE_CURRENT_SOURCE_DIR}" FILES ${acbm2ilbm_HEADERS} ${acbm2ilbm_SOURCES})
add_executable(acbm2ilbm ${acbm2ilbm_HEADERS} ${acbm2ilbm_SOURCES})
target_compile_definitions(acbm2ilbm PRIVATE "${acbm2ilbm_DEFINITIONS}")
target_link_libraries(acbm2ilbm PRIVATE ILBM::ILBM)


set(ilbm2acbm_HEADERS
  src/ilbm2acbm/ilbm2acbm.h
  )

set(ilbm2acbm_SOURCES
  src/ilbm2acbm/ilbm2acbm.c
  src/ilbm2acbm/main.c
  )

set(ilbm2acbm_DATAFILES
  src/ilbm2acbm/ilbm2acbm.vcxproj
  src/ilbm2acbm/ilbm2acbm.vcxproj.filters
  src/ilbm2acbm/Makefile.am
  )

set(ilbm2acbm_DEFINITIONS
  PACKAGE_NAME=\"${PROJECT_NAME}\"
  PACKAGE_VERSION=\"${PROJECT_VERSION}\"
  HAVE_GETOPT_H=${HAVE_GETOPT_H}
  )

source_group(TREE "${CMAKE_CURRENT_SOURCE_DIR}" FILES ${ilbm2acbm_HEADERS} ${ilbm2acbm_SOURCES})
add_executable(ilbm2acbm ${ilbm2acbm_HEADERS} ${ilbm2acbm_SOURCES})
target_compile_definitions(ilbm2acbm PRIVATE "${ilbm2acbm_DEFINITIONS}")
target_link_libraries(ilbm2acbm PRIVATE ILBM::ILBM)


set(ilbmpack_HEADERS
  src/ilbmpack/pack.h
  )

set(ilbmpack_SOURCES
  src/ilbmpack/pack.c
  src/ilbmpack/main.c
  )

set(ilbmpack_DATAFILES
  src/ilbmpack/ilbmpack.vcxproj
  src/ilbmpack/ilbmpack.vcxproj.filters
  src/ilbmpack/Makefile.am
  )

set(ilbmpack_DEFINITIONS
  PACKAGE_NAME=\"${PROJECT_NAME}\"
  PACKAGE_VERSION=\"${PROJECT_VERSION}\"
  HAVE_GETOPT_H=${HAVE_GETOPT_H}
  )

source_group(TREE "${CMAKE_CURRENT_SOURCE_DIR}" FILES ${ilbmpack_HEADERS} ${ilbmpack_SOURCES})
add_executable(ilbmpack ${ilbmpack_HEADERS} ${ilbmpack_SOURCES})
target_compile_definitions(ilbmpack PRIVATE "${ilbmpack_DEFINITIONS}")
target_link_libraries(ilbmpack PRIVATE ILBM::ILBM)


set(ilbmpp_HEADERS
  src/ilbmpp/pp.h
  )

set(ilbmpp_SOURCES
  src/ilbmpp/pp.c
  src/ilbmpp/main.c
  )

set(ilbmpp_DATAFILES
  src/ilbmpp/ilbmpp.vcxproj
  src/ilbmpp/ilbmpp.vcxproj.filters
  src/ilbmpp/Makefile.am
  )

set(ilbmpp_DEFINITIONS
  PACKAGE_NAME=\"${PROJECT_NAME}\"
  PACKAGE_VERSION=\"${PROJECT_VERSION}\"
  HAVE_GETOPT_H=${HAVE_GETOPT_H}
  )

source_group(TREE "${CMAKE_CURRENT_SOURCE_DIR}" FILES ${ilbmpp_HEADERS} ${ilbmpp_SOURCES})
add_executable(ilbmpp ${ilbmpp_HEADERS} ${ilbmpp_SOURCES})
target_compile_definitions(ilbmpp PRIVATE "${ilbmpp_DEFINITIONS}")
target_link_libraries(ilbmpp PRIVATE ILBM::ILBM)


install(TARGETS ilbm acbm2ilbm ilbm2acbm ilbmpack ilbmpp
  RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
  LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
  ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
  )

install(
  FILES "${CMAKE_CURRENT_BINARY_DIR}/libilbm.pc"
  DESTINATION "${CMAKE_INSTALL_LIBDIR}/pkgconfig"
  )

install(
  FILES ${ilbm_HEADERS}
  DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/libilbm"
  )
