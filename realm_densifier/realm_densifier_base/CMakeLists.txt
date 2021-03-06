cmake_minimum_required(VERSION 2.8.3)
project(realm_densifier_base)

IF (DENSIFIER_WITH_CUDA)
    add_definitions(-DUSE_CUDA)
    list(APPEND DENSIFIER_LIBS psl)
    list(APPEND DENSIFIER_SOURCES src/realm_densifier_lib/plane_sweep.cpp)
ENDIF()

find_package(catkin REQUIRED COMPONENTS
        cmake_modules
        realm_core
        realm_io
        ${DENSIFIER_LIBS}
        )

find_package(OpenCV 3.3.1 EXACT)

catkin_package(
        INCLUDE_DIRS include
        DEPENDS OpenCV ${DENSIFIER_LIBS}
        LIBRARIES ${PROJECT_NAME}
)

###################
## Build Library ##
###################

list(APPEND DENSIFIER_SOURCES
        src/realm_densifier_lib/densifier_factory.cpp
        src/realm_densifier_lib/densifier_settings_factory.cpp
        src/realm_densifier_lib/densifier_dummy.cpp
        src/realm_densifier_lib/sparse_depth.cpp
        )

## realm densifier
add_library(${PROJECT_NAME}
        ${DENSIFIER_SOURCES}
        )
target_include_directories(${PROJECT_NAME} PUBLIC
        include
        ${OpenCV_INCLUDE_DIRS}
        ${catkin_INCLUDE_DIRS}
        )
target_link_libraries(${PROJECT_NAME}
        ${OpenCV_LIBRARIES}
        ${catkin_LIBRARIES}
        )
add_definitions(-std=c++11)