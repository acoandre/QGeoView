set(CMAKE_CXX_STANDARD 11)
set(CMAKE_AUTOMOC ON)
set(GENERIC_LIB_VERSION "1.0.2")

include(GNUInstallDirs)

find_package(Qt5 REQUIRED COMPONENTS
    Core
    Gui
    Widgets
    Network
)

add_library(QGeoView SHARED
    include/QGeoView/QGVGlobal.h
    include/QGeoView/QGVProjection.h
    include/QGeoView/QGVProjectionEPSG3857.h
    include/QGeoView/QGVCamera.h
    include/QGeoView/QGVMap.h
    include/QGeoView/QGVMapQGItem.h
    include/QGeoView/QGVMapQGView.h
    include/QGeoView/QGVMapRubberBand.h
    include/QGeoView/QGVItem.h
    include/QGeoView/QGVDrawItem.h
    include/QGeoView/QGVLayer.h
    include/QGeoView/QGVImage.h
    include/QGeoView/QGVLayerTiles.h
    include/QGeoView/QGVLayerTilesOnline.h
    include/QGeoView/QGVLayerGoogle.h
    include/QGeoView/QGVLayerBing.h
    include/QGeoView/QGVLayerOSM.h
    include/QGeoView/QGVWidget.h
    include/QGeoView/QGVWidgetCompass.h
    include/QGeoView/QGVWidgetScale.h
    include/QGeoView/QGVWidgetZoom.h
    include/QGeoView/QGVWidgetText.h
    src/QGVGlobal.cpp
    src/QGVProjection.cpp
    src/QGVProjectionEPSG3857.cpp
    src/QGVCamera.cpp
    src/QGVMap.cpp
    src/QGVMapQGItem.cpp
    src/QGVMapQGView.cpp
    src/QGVMapRubberBand.cpp
    src/QGVItem.cpp
    src/QGVDrawItem.cpp
    src/QGVLayer.cpp
    src/QGVImage.cpp
    src/QGVLayerTiles.cpp
    src/QGVLayerTilesOnline.cpp
    src/QGVLayerGoogle.cpp
    src/QGVLayerBing.cpp
    src/QGVLayerOSM.cpp
    src/QGVWidget.cpp
    src/QGVWidgetCompass.cpp
    src/QGVWidgetScale.cpp
    src/QGVWidgetZoom.cpp
    src/QGVWidgetText.cpp
)

target_include_directories(QGeoView
    PUBLIC
        $<INSTALL_INTERFACE:include>
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    PRIVATE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/QGeoView>
)

target_compile_definitions(QGeoView
    PRIVATE
        QGV_EXPORT
)

target_link_libraries(QGeoView
    PRIVATE
        Qt5::Core
        Qt5::Gui
        Qt5::Widgets
        Qt5::Network
)

install(TARGETS QGeoView
        EXPORT ${CMAKE_PROJECT_NAME}Targets
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}/static
)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/
        DESTINATION include
        FILES_MATCHING PATTERN "*.h*"
)


include(CMakePackageConfigHelpers)
set(TARGETS_EXPORT_NAME "${PROJECT_NAME}Targets")
configure_package_config_file(
  "Config.cmake.in"
  "${CMAKE_BINARY_DIR}/${CMAKE_PROJECT_NAME}Config.cmake"
  INSTALL_DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/${CMAKE_PROJECT_NAME}"
)
write_basic_package_version_file(
  "${CMAKE_BINARY_DIR}/${CMAKE_PROJECT_NAME}ConfigVersion.cmake"
  VERSION ${GENERIC_LIB_VERSION}
  COMPATIBILITY SameMajorVersion
)
install(FILES
        ${CMAKE_BINARY_DIR}/${CMAKE_PROJECT_NAME}Config.cmake
        ${CMAKE_BINARY_DIR}/${CMAKE_PROJECT_NAME}ConfigVersion.cmake
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${CMAKE_PROJECT_NAME}
)
install(EXPORT ${CMAKE_PROJECT_NAME}Targets
        NAMESPACE QGeoView::
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${CMAKE_PROJECT_NAME}
)