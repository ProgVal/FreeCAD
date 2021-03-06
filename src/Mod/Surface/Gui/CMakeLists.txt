if(MSVC)
    add_definitions(-DHAVE_ACOSH -DHAVE_ATANH -DHAVE_ASINH)
else(MSVC)
    add_definitions(-DHAVE_LIMITS_H -DHAVE_CONFIG_H)
endif(MSVC)

include_directories(
    ${CMAKE_BINARY_DIR}
    ${CMAKE_SOURCE_DIR}/src
    ${CMAKE_BINARY_DIR}/src
    ${CMAKE_CURRENT_BINARY_DIR}
    ${Boost_INCLUDE_DIRS}
    ${COIN3D_INCLUDE_DIRS}
    ${OCC_INCLUDE_DIR}
    ${PYTHON_INCLUDE_DIRS}
    ${XercesC_INCLUDE_DIRS}
    ${ZLIB_INCLUDE_DIR}
)

set(SurfaceGui_LIBS
    Surface
    PartGui
    FreeCADGui
)

set(SurfaceGui_MOC_HDRS
    TaskFilling.h
    TaskFillingUnbound.h
    TaskFillingVertex.h
    TaskGeomFillSurface.h
)
fc_wrap_cpp(SurfaceGui_MOC_SRCS ${SurfaceGui_MOC_HDRS})
SOURCE_GROUP("Moc" FILES ${SurfaceGui_MOC_SRCS})

if (BUILD_QT5)
    qt5_add_resources(SurfaceGui_QRC_SRCS Resources/Surface.qrc)
else()
    qt4_add_resources(SurfaceGui_QRC_SRCS Resources/Surface.qrc)
endif()

SET(SurfaceGui_UIC_SRCS
    TaskFilling.ui
    TaskFillingUnbound.ui
    TaskFillingVertex.ui
    TaskGeomFillSurface.ui
)

if (BUILD_QT5)
    qt5_wrap_ui(SurfaceGui_UIC_HDRS ${SurfaceGui_UIC_SRCS})
else()
    qt4_wrap_ui(SurfaceGui_UIC_HDRS ${SurfaceGui_UIC_SRCS})
endif()

SET(SurfaceGui_SRCS
    ${SurfaceGui_QRC_SRCS}
    ${SurfaceGui_UIC_HDRS}
    TaskFilling.cpp
    TaskFilling.h
    TaskFillingUnbound.cpp
    TaskFillingUnbound.h
    TaskFillingVertex.cpp
    TaskFillingVertex.h
    TaskGeomFillSurface.cpp
    TaskGeomFillSurface.h
    AppSurfaceGui.cpp
    Command.cpp
    PreCompiled.cpp
    PreCompiled.h
    Workbench.cpp
    Workbench.h
#    ViewProviderCut.cpp
#    ViewProviderCut.h
)

link_directories(${OCC_LIBRARY_DIR})
add_library(SurfaceGui SHARED ${SurfaceGui_SRCS})
target_link_libraries(SurfaceGui ${SurfaceGui_LIBS})

fc_target_copy_resource(SurfaceGui 
    ${CMAKE_SOURCE_DIR}/src/Mod/Surface
    ${CMAKE_BINARY_DIR}/Mod/Surface
    InitGui.py)

SET_BIN_DIR(SurfaceGui SurfaceGui /Mod/Surface)
SET_PYTHON_PREFIX_SUFFIX(SurfaceGui)
    
install(TARGETS SurfaceGui DESTINATION ${CMAKE_INSTALL_LIBDIR})
