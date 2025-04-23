cmake_minimum_required(VERSION 3.10 FATAL_ERROR)
set(PROJECT lightdm-qt5-greeter)
project(${PROJECT})

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)

file(GLOB SRCS *.cpp *ui *moc)
# message(STATUS "SRCS ${SRCS}")

find_package(Qt5 5.10.0 CONFIG REQUIRED COMPONENTS Core Gui Widgets LinguistTools)

QT5_ADD_RESOURCES(RSCS lightdm-qt5-greeter.qrc)


include(FindPkgConfig)
pkg_check_modules(LIGHTDM_QT liblightdm-qt5-3)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}
                      ${CMAKE_CURRENT_BINARY_DIR}
                      ${LIGHTDM_QT_INCLUDE_DIRS}
)

add_executable(lightdm-qt5-greeter ${SRCS} ${RSCS})

target_link_libraries(lightdm-qt5-greeter lightdm-qt5-3 Qt5::Widgets Qt5::Gui Qt5::Core)

file(GLOB TS_FILES ${CMAKE_CURRENT_SOURCE_DIR}/translations/*.ts)
qt5_add_translation(QM_FILES ${TS_FILES})
add_custom_target(translations ALL DEPENDS ${QM_FILES})

install(TARGETS ${PROJECT} RUNTIME DESTINATION bin)
install(FILES   lightdm-qt5-greeter.desktop  DESTINATION /usr/share/xgreeters)
install(FILES   ${QM_FILES}  DESTINATION /usr/share/lightdm-qt5-greeter/translations)

if (NOT KEEP_CONFIGS)
    install(FILES lightdm-qt5-greeter.conf DESTINATION /etc/lightdm)
endif()

