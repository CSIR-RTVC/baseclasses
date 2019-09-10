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
install(TARGETS baseclasses
	EXPORT BaseClassesTargets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
)
export(TARGETS baseclasses
       NAMESPACE Baseclasses::
       FILE "${CMAKE_CURRENT_BINARY_DIR}/baseclassesConfig.cmake"
)
install(EXPORT BaseClassesTargets
	DESTINATION lib/cmake/baseclasses
	FILE baseclassesConfig.cmake
	NAMESPACE BaseClasses::
)
target_include_directories(baseclasses
    PUBLIC 
        $<INSTALL_INTERFACE:include>    
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
		${PROJECT_SOURCE_DIR}/include
)
