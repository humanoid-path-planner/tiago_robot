cmake_minimum_required(VERSION 2.8.3)
project(tiago_description)

include(cmake/xacro.cmake)

find_package(catkin REQUIRED)

catkin_package()

foreach(dir gazebo Media meshes robots urdf srdf)
    install(DIRECTORY ${dir}/
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/${dir})
endforeach(dir)

SET(TYPES iron steel airbus)

MAKE_DIRECTORY(${CMAKE_BINARY_DIR}/robots)
FOREACH(type ${TYPES})
  GENERATE_URDF_FILE(robots/tiago_${type}.urdf "urdf")
  ADD_CUSTOM_COMMAND(
    OUTPUT ${CMAKE_BINARY_DIR}/robots/tiago_${type}.urdf
    COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_BINARY_DIR}/robots/tiago_${type}.urdf.urdf ${CMAKE_BINARY_DIR}/robots/tiago_${type}.urdf
    MAIN_DEPENDENCY ${CMAKE_BINARY_DIR}/robots/tiago_${type}.urdf.urdf)
  LIST(APPEND ALL_RENAMED_URDF ${CMAKE_BINARY_DIR}/robots/tiago_${type}.urdf)
  INSTALL(FILES
    ${CMAKE_BINARY_DIR}/robots/tiago_${type}.urdf
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/urdf)
ENDFOREACH(type ${TYPES})

ADD_CUSTOM_TARGET (generate_urdf_file ALL DEPENDS ${ALL_RENAMED_URDF})
