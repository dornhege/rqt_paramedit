cmake_minimum_required(VERSION 2.8.3)
project(rqt_paramedit)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  qt_paramedit
  rqt_gui_cpp
  rqt_gui)

find_package(Qt5 COMPONENTS Core Gui Widgets REQUIRED)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

set(rqt_paramedit_INCLUDE_DIRECTORIES
    include
    ${CMAKE_CURRENT_BINARY_DIR}
    )

catkin_package(
    INCLUDE_DIRS include
    LIBRARIES ${PROJECT_NAME}
    CATKIN_DEPENDS roscpp qt_paramedit rqt_gui_cpp rqt_gui
)
catkin_python_setup()

#set(rqt_paramedit_HDRS
#    include/rqt_paramedit/param_edit.h
#    src/rqt_paramedit/param_root_chooser.h
#    )
#qt4_wrap_cpp(rqt_paramedit_MOCS ${rqt_paramedit_HDRS})

include_directories(${rqt_paramedit_INCLUDE_DIRECTORIES} ${catkin_INCLUDE_DIRS})
add_library(${PROJECT_NAME}
    src/rqt_paramedit/param_edit.cpp
    src/rqt_paramedit/param_root_chooser.cpp
    include/rqt_paramedit/param_edit.h
    src/rqt_paramedit/param_root_chooser.h
    src/rqt_paramedit/param_root_chooser.ui)
target_link_libraries(${PROJECT_NAME}
    Qt5::Core
    Qt5::Gui
    Qt5::Widgets
    ${catkin_LIBRARIES})

find_package(class_loader REQUIRED)
class_loader_hide_library_symbols(${PROJECT_NAME})

install(FILES rqt_paramedit_plugin.xml
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

install(TARGETS ${PROJECT_NAME}
    ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
)

install(PROGRAMS scripts/rqt_paramedit
    DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
)
install(PROGRAMS scripts/rqt_paramedit
    DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY include/${PROJECT_NAME}
    DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

