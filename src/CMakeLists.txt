############################################################################
# CMakeLists.txt
# Copyright (C) 2017  Belledonne Communications, Grenoble France
#
############################################################################
#
# This program is free software; you can redistribute it and/or
# modify it under the terms of the GNU General Public License
# as published by the Free Software Foundation; either version 2
# of the License, or (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301, USA.
#
############################################################################

set(LINPHONE_CXX_OBJECTS_PRIVATE_HEADER_FILES
	cpim/cpim.h
	cpim/header/cpim-core-headers.h
	cpim/header/cpim-generic-header.h
	cpim/header/cpim-header-p.h
	cpim/header/cpim-header.h
	cpim/message/cpim-message.h
	cpim/parser/cpim-grammar.h
	cpim/parser/cpim-parser.h
	db/abstract/abstract-db-p.h
	db/abstract/abstract-db.h
	db/events-db.h
	db/provider/db-session-p.h
	db/provider/db-session-provider.h
	db/provider/db-session.h
	enums.h
	event/call-event.h
	event/event.h
	event/message-event.h
	logger/logger.h
	message/message.h
	object/clonable-object-p.h
	object/clonable-object.h
	object/object-p.h
	object/object.h
	object/singleton.h
	utils/general.h
	utils/utils.h
)

set(LINPHONE_CXX_OBJECTS_SOURCE_FILES
	cpim/header/cpim-core-headers.cpp
	cpim/header/cpim-generic-header.cpp
	cpim/header/cpim-header.cpp
	cpim/message/cpim-message.cpp
	cpim/parser/cpim-grammar.cpp
	cpim/parser/cpim-parser.cpp
	db/abstract/abstract-db.cpp
	db/events-db.cpp
	db/provider/db-session-provider.cpp
	db/provider/db-session.cpp
	event/call-event.cpp
	event/event.cpp
	event/message-event.cpp
	logger/logger.cpp
	message/message.cpp
	object/clonable-object.cpp
	object/object.cpp
	utils/general.cpp
	utils/utils.cpp
)

set(LINPHONE_CXX_OBJECTS_DEFINITIONS "-DLIBLINPHONE_EXPORTS")
set(LINPHONE_CXX_OBJECTS_INCLUDE_DIRS ${BELR_INCLUDE_DIRS})

if(SOCI_FOUND)
	list(APPEND LINPHONE_CXX_OBJECTS_INCLUDE_DIRS ${SOCI_INCLUDE_DIRS} ${SOCI_MYSQL_INCLUDES})
	add_definitions(-DSOCI_ENABLED)
endif()

set(LINPHONE_PRIVATE_HEADER_FILES)
foreach(header ${LINPHONE_CXX_OBJECTS_PRIVATE_HEADER_FILES})
	list(APPEND LINPHONE_PRIVATE_HEADER_FILES "${CMAKE_CURRENT_SOURCE_DIR}/${header}")
endforeach()
set(LINPHONE_PRIVATE_HEADER_FILES ${LINPHONE_PRIVATE_HEADER_FILES} PARENT_SCOPE)

bc_apply_compile_flags(LINPHONE_CXX_OBJECTS_SOURCE_FILES STRICT_OPTIONS_CPP STRICT_OPTIONS_CXX)

if(ENABLE_STATIC)
	add_library(linphone-cxx-objects-static OBJECT ${LINPHONE_CXX_OBJECTS_PRIVATE_HEADER_FILES} ${LINPHONE_CXX_OBJECTS_SOURCE_FILES})
	target_compile_definitions(linphone-cxx-objects-static PRIVATE ${LINPHONE_CXX_OBJECTS_DEFINITIONS})
	target_include_directories(linphone-cxx-objects-static SYSTEM PRIVATE ${LINPHONE_CXX_OBJECTS_INCLUDE_DIRS} ${LINPHONE_INCLUDE_DIRS})
endif()

if(ENABLE_SHARED)
	add_library(linphone-cxx-objects OBJECT ${LINPHONE_CXX_OBJECTS_PRIVATE_HEADER_FILES} ${LINPHONE_CXX_OBJECTS_SOURCE_FILES})
	target_compile_definitions(linphone-cxx-objects PRIVATE ${LINPHONE_CXX_OBJECTS_DEFINITIONS})
	target_include_directories(linphone-cxx-objects SYSTEM PRIVATE ${LINPHONE_CXX_OBJECTS_INCLUDE_DIRS} ${LINPHONE_INCLUDE_DIRS})
	target_compile_options(linphone-cxx-objects PRIVATE "-fPIC")
endif()
