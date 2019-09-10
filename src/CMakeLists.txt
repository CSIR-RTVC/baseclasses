# CMakeLists.txt in baseclasses/src dir

SET(BASE_CLASSES_HDRS
    ../include/amextra.h
    ../include/amfilter.h
    ../include/cache.h
    ../include/checkbmi.h
    ../include/combase.h
    ../include/cprop.h
    ../include/ctlutil.h
    ../include/ddmm.h
    ../include/dllsetup.h
    ../include/dxmperf.h
    ../include/fourcc.h
    ../include/measure.h
    ../include/msgthrd.h
    ../include/mtype.h
    ../include/outputq.h
    ../include/perflog.h
    ../include/perfstruct.h
    ../include/pstream.h
    ../include/pullpin.h
    ../include/refclock.h
    ../include/reftime.h
    ../include/renbase.h
    ../include/schedule.h
    ../include/seekpt.h
    ../include/source.h
    ../include/streams.h
    ../include/strmctl.h
    ../include/sysclock.h
    ../include/transfrm.h
    ../include/transip.h
    ../include/videoctl.h
    ../include/vtrans.h
    ../include/winctrl.h
    ../include/winutil.h
    ../include/wxdebug.h
    ../include/wxlist.h
    ../include/wxutil.h
    )

SET(BASE_CLASSES_SRCS
    amextra.cpp
    amfilter.cpp
    amvideo.cpp
    arithutil.cpp
    combase.cpp
    cprop.cpp
    ctlutil.cpp
    ddmm.cpp
    dllentry.cpp
    dllsetup.cpp
    mtype.cpp
    outputq.cpp
    perflog.cpp
    pstream.cpp
    pullpin.cpp
    refclock.cpp
    renbase.cpp
    schedule.cpp
    seekpt.cpp
    source.cpp
    strmctl.cpp
    sysclock.cpp
    transfrm.cpp
    transip.cpp
    videoctl.cpp
    vtrans.cpp
    winctrl.cpp
    winutil.cpp
    wxdebug.cpp
    wxlist.cpp
    wxutil.cpp   
    )

ADD_LIBRARY(baseclasses STATIC ${BASE_CLASSES_SRCS} ${BASE_CLASSES_HDRS})
#Add an alias so that library can be used inside the build tree, e.g. when testing
add_library(BaseClasses::baseclasses ALIAS baseclasses)

#Set target properties
target_include_directories(baseclasses
    PUBLIC
        $<INSTALL_INTERFACE:include>
	$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
)

target_compile_features(baseclasses PRIVATE cxx_auto_type)
target_compile_options(baseclasses PRIVATE $<$<CXX_COMPILER_ID:GNU>:-Wall>)

target_link_libraries(baseclasses
    PUBLIC
    PRIVATE
    winmm
)

##############################################
# Installation instructions

include(GNUInstallDirs)
set(INSTALL_CONFIGDIR ${CMAKE_INSTALL_LIBDIR}/cmake/BaseClasses)

install(TARGETS baseclasses
    EXPORT baseclasses-targets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

#This is required so that the exported target has the name JSONUtils and not jsonutils
set_target_properties(baseclasses PROPERTIES EXPORT_NAME BaseClasses)

install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

#Export the targets to a script
install(EXPORT baseclasses-targets
  FILE
  BaseClassesTargets.cmake
  NAMESPACE
  BaseClasses::
  DESTINATION
    ${INSTALL_CONFIGDIR}
)

#Create a ConfigVersion.cmake file
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/BaseClassesConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(${CMAKE_CURRENT_LIST_DIR}/../cmake/BaseClassesConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/BaseClassesConfig.cmake
    INSTALL_DESTINATION ${INSTALL_CONFIGDIR}
)

#Install the config, configversion and custom find modules
install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/BaseClassesConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/BaseClassesConfigVersion.cmake
    DESTINATION ${INSTALL_CONFIGDIR}
)

##############################################

export(EXPORT baseclasses-targets FILE ${CMAKE_CURRENT_BINARY_DIR}/baseclassesTargets.cmake NAMESPACE BaseClasses::)

#Register package in user's package registry
export(PACKAGE baseclasses)


