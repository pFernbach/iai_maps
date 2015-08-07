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
project(iai_maps)

find_package(catkin REQUIRED COMPONENTS
  xacro robot_state_chain_publisher geometry_msgs rostime sensor_msgs tf)

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
  Media/models/glasses
  Media/models/chair
  Media/models/cnc
  Media/models/cash_register
  Media/models/wine_cooler
  Media/models/test_glasses
  room/test
  room/colorTest
  room/cnc_area
  room/bio_area
  room/worktables
  room/allInOne
  room/kitchen_area
  room/lab_macros
  room/shopping_area
	room/tunnel
	room/tunnelSmall
	room/room
	room/labyrinth
	room/labyrinth2
  room/grid
  room/crack
  room/hole
  room/engine_noCable
  room/funnel
  room/powerplant
  room/powerplant_notop
  )

SET(XACRO_SRDF_FILES
  room/srdf/kitchen_area
	room/srdf/test
	room/srdf/shopping_area
	room/srdf/worktables
	room/srdf/lab_macros
	room/srdf/bio_area
	room/srdf/cnc_area
	room/srdf/allInOne
	room/srdf/tunnel
	room/srdf/room
	room/srdf/labyrinth
	room/srdf/labyrinth2
	room/srdf/tunnelSmall
  room/srdf/grid
  room/srdf/crack
  room/srdf/hole
  room/srdf/engine_noCable
  room/srdf/funnel
  room/srdf/powerplant
  room/srdf/powerplant_notop
  )

foreach(it ${XACRO_FILES})
  config_files(${it}.xacro)
  get_filename_component(basepath ${it} PATH)
  get_filename_component(basename ${it} NAME)
  set (basepath ${CMAKE_BINARY_DIR}/${basepath})

  message(" processing xacro file :${basepath}/${basename}.xacro")
  set(expanded_file "${basepath}/${basename}.urdf")
  message("  produce expanded file :"${expanded_file})
  
  execute_process(
    COMMAND ${_xacro_py} -o ${expanded_file} ${basepath}/${basename}.xacro
    )
  set(URDF_FILES ${URDF_FILES} ${expanded_file})
endforeach(it)

foreach(it ${XACRO_SRDF_FILES})
  config_files(${it}.xacro)
  get_filename_component(basepath ${it} PATH)
  get_filename_component(basename ${it} NAME)
  set (basepath ${CMAKE_BINARY_DIR}/${basepath})

  message(" processing xacro file :${basepath}/${basename}.xacro")
  set(expanded_file "${basepath}/${basename}.srdf")
  message("  produce expanded file :"${expanded_file})

  execute_process(
    COMMAND ${_xacro_py} -o ${expanded_file} ${basepath}/${basename}.xacro
    )
  set(SRDF_FILES ${SRDF_FILES} ${expanded_file})
endforeach(it)

INSTALL(FILES
  ${CMAKE_BINARY_DIR}/room/kitchen_area.urdf
	${CMAKE_BINARY_DIR}/room/test.urdf
	${CMAKE_BINARY_DIR}/room/shopping_area.urdf
	${CMAKE_BINARY_DIR}/room/worktables.urdf
	${CMAKE_BINARY_DIR}/room/lab_macros.urdf
	${CMAKE_BINARY_DIR}/room/bio_area.urdf
	${CMAKE_BINARY_DIR}/room/cnc_area.urdf
	${CMAKE_BINARY_DIR}/room/allInOne.urdf
	${CMAKE_BINARY_DIR}/room/tunnelSmall.urdf
	${CMAKE_BINARY_DIR}/room/tunnel.urdf
	${CMAKE_BINARY_DIR}/room/room.urdf
	${CMAKE_BINARY_DIR}/room/labyrinth.urdf
	${CMAKE_BINARY_DIR}/room/labyrinth2.urdf
	${CMAKE_BINARY_DIR}/room/backward_easy.urdf
	${CMAKE_BINARY_DIR}/room/grid.urdf
	${CMAKE_BINARY_DIR}/room/hole.urdf
	${CMAKE_BINARY_DIR}/room/crack.urdf
  ${CMAKE_BINARY_DIR}/room/engine_noCable.urdf
  ${CMAKE_BINARY_DIR}/room/funnel.urdf
  ${CMAKE_BINARY_DIR}/room/powerplant.urdf
  ${CMAKE_BINARY_DIR}/room/powerplant_notop.urdf
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/urdf
  )

INSTALL(FILES
  ${CMAKE_BINARY_DIR}/room/srdf/kitchen_area.srdf
	${CMAKE_BINARY_DIR}/room/srdf/test.srdf
	${CMAKE_BINARY_DIR}/room/srdf/shopping_area.srdf
	${CMAKE_BINARY_DIR}/room/srdf/worktables.srdf
	${CMAKE_BINARY_DIR}/room/srdf/lab_macros.srdf
	${CMAKE_BINARY_DIR}/room/srdf/bio_area.srdf
	${CMAKE_BINARY_DIR}/room/srdf/cnc_area.srdf
	${CMAKE_BINARY_DIR}/room/srdf/allInOne.srdf
	${CMAKE_BINARY_DIR}/room/srdf/tunnel.srdf
	${CMAKE_BINARY_DIR}/room/srdf/tunnelSmall.srdf
	${CMAKE_BINARY_DIR}/room/srdf/room.srdf
	${CMAKE_BINARY_DIR}/room/srdf/labyrinth.srdf
	${CMAKE_BINARY_DIR}/room/srdf/labyrinth2.srdf
	${CMAKE_BINARY_DIR}/room/srdf/backward_easy.srdf
	${CMAKE_BINARY_DIR}/room/srdf/grid.srdf
	${CMAKE_BINARY_DIR}/room/srdf/hole.srdf
	${CMAKE_BINARY_DIR}/room/srdf/crack.srdf
  ${CMAKE_BINARY_DIR}/room/srdf/engine_noCable.srdf
  ${CMAKE_BINARY_DIR}/room/srdf/funnel.srdf
  ${CMAKE_BINARY_DIR}/room/srdf/powerplant.srdf
  ${CMAKE_BINARY_DIR}/room/srdf/powerplant_notop.srdf
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
  meshs/backward_easy.dae
  meshs/backward_easy.stl
  meshs/grid.stl
  meshs/grid_light.stl
  meshs/crack.stl
  meshs/hole_big.stl
  meshs/35HP_ENGINE_noCable_easy.stl
  meshs/cable_funnel_light.stl
  meshs/powerplant_simplification.stl
  meshs/powerplant_noTop.stl
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/meshs
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
