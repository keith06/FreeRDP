# FreeRDP: A Remote Desktop Protocol Client
# libfreerdp-gdi cmake build script
#
# Copyright 2012 Marc-Andre Moreau <marcandre.moreau@gmail.com>
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

set(MODULE_NAME "freerdp-gdi")
set(MODULE_PREFIX "FREERDP_GDI")

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

set(${MODULE_PREFIX}_SRCS
	8bpp.c
	16bpp.c
	32bpp.c
	bitmap.c
	brush.c
	clipping.c
	dc.c
	drawing.c
	line.c
	palette.c
	pen.c
	region.c
	shape.c
	graphics.c
	graphics.h
	gdi.c
	gdi.h)

if(WITH_MONOLITHIC_BUILD)
	add_library(${MODULE_NAME} OBJECT ${${MODULE_PREFIX}_SRCS})
else()
	add_library(${MODULE_NAME} ${${MODULE_PREFIX}_SRCS})
endif()

set_target_properties(${MODULE_NAME} PROPERTIES VERSION ${FREERDP_VERSION_FULL} SOVERSION ${FREERDP_VERSION} PREFIX "lib")

if(WITH_MONOLITHIC_BUILD)
	set(FREERDP_LIBS ${FREERDP_LIBS} ${${MODULE_PREFIX}_LIBS} PARENT_SCOPE)
else()
	set(${MODULE_PREFIX}_LIBS ${${MODULE_PREFIX}_LIBS}
		freerdp-core
		freerdp-cache
		freerdp-codec)
	
	target_link_libraries(${MODULE_NAME} ${${MODULE_PREFIX}_LIBS})
	install(TARGETS ${MODULE_NAME} DESTINATION ${CMAKE_INSTALL_LIBDIR})
endif()

set_property(TARGET ${MODULE_NAME} PROPERTY FOLDER "FreeRDP")
