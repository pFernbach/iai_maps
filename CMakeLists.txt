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
project(iai_maps)

find_package(catkin REQUIRED COMPONENTS
  xacro robot_state_chain_publisher geometry_msgs rostime sensor_msgs tf)

catkin_package()

macro(generate_urdf input)
  string(REPLACE ".urdf.xml" ".urdf" "output" "${input}")
  execute_process(
    COMMAND rosrun xacro xacro.py -o ${CMAKE_BINARY_DIR}/${output}
    ${CMAKE_SOURCE_DIR}/${input})
endmacro()

file(MAKE_DIRECTORY ${CMAKE_BINARY_DIR}/room)
generate_urdf (room/kitchen_area.urdf.xml)

INSTALL(FILES
  defs/util_defs.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/defs
  )

INSTALL(FILES
  defs/ias_kitchen/materials.xml
  defs/ias_kitchen/furniture_defs.xml
  defs/ias_kitchen/oven_block.xml
  defs/ias_kitchen/sink_block.xml
  defs/ias_kitchen/small_table.xml
  defs/ias_kitchen/fridge_block.xml
  defs/ias_kitchen/table.xml
  defs/ias_kitchen/cuboid.xml
  defs/ias_kitchen/shopping_block.xml
  defs/ias_kitchen/island_block.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/defs/ias_kitchen
  )

INSTALL(FILES
  Media/models/glasses.urdf.xml
  Media/models/chair.urdf.xml
  Media/models/cnc.urdf.xml
  Media/models/cash_register.urdf.xml
  Media/models/wine_cooler.urdf.xml
  Media/models/test_glasses.urdf.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/Media/models
  )

INSTALL(FILES
  room/test.urdf.xml
  room/colorTest.urdf.xml
  room/cnc_area.urdf.xml
  room/bio_area.urdf.xml
  room/worktables.urdf.xml
  room/allInOne.urdf.xml
  room/kitchen_area.urdf.xml
  room/lab_macros.xml
  room/room.urdf.xml
  room/shopping_area.urdf.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/room
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
