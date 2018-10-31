# Copyright (c) 2014 CNRS
# Author: Florent Lamiraux
#
# this file is free software: you can redistribute it
# and/or modify it under the terms of the GNU Lesser General Public
# License as published by the Free Software Foundation, either version
# 3 of the License, or (at your option) any later version.
#
# this file is distributed in the hope that it will be
# useful, but WITHOUT ANY WARRANTY; without even the implied warranty
# of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
# General Lesser Public License for more details.  You should have
# received a copy of the GNU Lesser General Public License along with
# this file.  If not, see
# <http://www.gnu.org/licenses/>.

cmake_minimum_required(VERSION 2.6)
include(cmake/portability.cmake)
include(cmake/xacro.cmake)
project(iai_maps)

find_package(catkin REQUIRED)

catkin_package()

# build urdf files using xacro
set(XACRO_FILES
  defs/util_defs
  defs/ias_kitchen/materials
  defs/ias_kitchen/furniture_defs
  defs/ias_kitchen/oven_block
  defs/ias_kitchen/sink_block
  defs/ias_kitchen/small_table
  defs/ias_kitchen/fridge_block
  defs/ias_kitchen/table
  defs/ias_kitchen/cuboid
  defs/ias_kitchen/shopping_block
  defs/ias_kitchen/island_block
  defs/ias_kitchen/staircase
  room/kitchen_area
  room/staircase
  room/floor_with_bar
  room/table
  room/table_with_pillar
  room/lab_macros
  room/maze
  room/maze2
  room/room
  )

SET(XACRO_SRDF_FILES
  room/srdf/kitchen_area
  room/srdf/staircase
  room/srdf/floor_with_bar
  room/srdf/table
  room/srdf/table_with_pillar
  room/srdf/maze
  room/srdf/maze2
  room/srdf/room

  )

foreach(it ${XACRO_FILES})
 GENERATE_URDF_FILE (${it} "urdf")
endforeach(it)

foreach(it ${XACRO_SRDF_FILES})
  GENERATE_URDF_FILE(${it} "srdf")
endforeach(it)

ADD_CUSTOM_TARGET (generate_urdf_file ALL DEPENDS ${ALL_GENERATED_URDF})

INSTALL(FILES
  ${CMAKE_BINARY_DIR}/room/kitchen_area.urdf
  ${CMAKE_BINARY_DIR}/room/staircase.urdf
  ${CMAKE_BINARY_DIR}/room/floor_with_bar.urdf
  ${CMAKE_BINARY_DIR}/room/table.urdf
  ${CMAKE_BINARY_DIR}/room/table_with_pillar.urdf
  ${CMAKE_BINARY_DIR}/room/maze.urdf
  ${CMAKE_BINARY_DIR}/room/maze2.urdf
  ${CMAKE_BINARY_DIR}/room/room.urdf
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/urdf
  )

INSTALL(FILES
  ${CMAKE_BINARY_DIR}/room/srdf/kitchen_area.srdf
  ${CMAKE_BINARY_DIR}/room/srdf/staircase.srdf
  ${CMAKE_BINARY_DIR}/room/srdf/floor_with_bar.srdf
  ${CMAKE_BINARY_DIR}/room/srdf/table.srdf
  ${CMAKE_BINARY_DIR}/room/srdf/table_with_pillar.srdf
  ${CMAKE_BINARY_DIR}/room/srdf/maze.srdf
  ${CMAKE_BINARY_DIR}/room/srdf/maze2.srdf
  ${CMAKE_BINARY_DIR}/room/srdf/room.srdf
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/srdf
  )

INSTALL(FILES
  ${CMAKE_BINARY_DIR}/room/staircase.urdf
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/urdf
  )

INSTALL(FILES
  ${CMAKE_BINARY_DIR}/room/srdf/staircase.srdf
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/srdf
  )

INSTALL(FILES
  Media/models/meshes/glasses/__Color_A05_4.png
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/Media/models/meshes/glasses
)

INSTALL(FILES
  Media/materials/textures/ganz_rechts_neben_kuecheninsel.png
  Media/materials/textures/hinter_shopschraenken_an_saeule.png
  Media/materials/textures/wallpaper_ai_unihb.png
  Media/materials/textures/stove.png
  Media/materials/textures/ganz_links_vorn_bei_biolab.png
  Media/materials/textures/wallpaper2.png
  Media/materials/textures/ganz_links_neben_roboterplakat.png
  Media/materials/textures/auf_saeule_bei_shop.png
  Media/materials/textures/wallpaper_project_logos.png
  Media/materials/textures/wallpaper.png
  Media/materials/textures/sink.png
  Media/materials/textures/mittig_neben_roboterplakat.png
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/Media/materials/textures
)

INSTALL(FILES
  rviz_configs/iai_kitchen_only.rviz
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/rviz_configs
  )

INSTALL(FILES
  launch/kitchen_with_state_publisher.launch
  launch/iai_kitchen_in_rviz.launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
  )
