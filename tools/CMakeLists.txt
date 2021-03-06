#         OpenLase - a realtime laser graphics toolkit
#
# Copyright (C) 2009-2011 Hector Martin "marcan" <hector@marcansoft.com>
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 2 or version 3.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301  USA
#

include_directories (${CMAKE_SOURCE_DIR}/include)
link_directories (${CMAKE_BINARY_DIR}/libol)
include_directories (${JACK_INCLUDE_DIR})

add_executable(openlase-playilda playilda.c)
target_link_libraries(openlase-playilda ${JACK_LIBRARIES})

install (TARGETS openlase-playilda RUNTIME DESTINATION bin)

if(FFMPEG_FOUND)
  include_directories(${FFMPEG_INCLUDE_DIR})
  add_executable(openlase-playvid playvid.c)
  target_link_libraries(openlase-playvid openlase ${FFMPEG_LIBRARIES}) 

  if(APPLE)
    target_link_libraries(openlase-playvid "-framework CoreFoundation" "-framework VideoDecodeAcceleration" "-framework QuartzCore" bz2 z)
  endif(APPLE)
  install (TARGETS openlase-playvid RUNTIME DESTINATION bin)
else()
  message(STATUS "Will NOT build playvid (FFmpeg missing)")
endif()

if(OPENGL_FOUND AND GLUT_FOUND)
  add_executable(openlase-simulator simulator.c)
  include_directories(${OPENGL_INCLUDE_DIRS} ${GLUT_INCLUDE_DIRS})
  include_directories (${JACK_INCLUDE_DIR})
  target_link_libraries(openlase-simulator m ${OPENGL_LIBRARIES} ${GLUT_LIBRARY} ${JACK_LIBRARIES})
  install(TARGETS openlase-simulator RUNTIME DESTINATION bin)
else()
  message(STATUS "Will NOT build simulator (OpenGL or GLUT missing)")
endif()

add_subdirectory(qplayvid)
