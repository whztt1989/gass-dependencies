cmake_minimum_required(VERSION 2.6)
set(LIB_NAME PagedGeometry)
set(LIB_TYPE STATIC)

project(${LIB_NAME})
set(CMAKE_DEBUG_POSTFIX  "_d" CACHE STRING "add a postfix, usually d on windows")
#add_definitions()

set(OGRE_DIR $ENV{OGRE_HOME} CACHE PATH "Ogre 3D folder")

# Set where to find ogre Cmake files on unix
if (UNIX)
  #set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "/usr/local/lib/OGRE/cmake/" "/usr/lib/OGRE/cmake/")
   set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${OGRE_DIR}/CMake /usr/share/OGRE/cmake/modules)
else (UNIX)
  set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${OGRE_DIR}/CMake)
  set(BOOST_INCLUDE_DIR $ENV{BOOST_ROOT} CACHE PATH "Boost folder")
endif ()
find_package(OGRE)



set(LIBRARY_OUTPUT_PATH lib/${CMAKE_BUILD_TYPE})

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include 
	${CMAKE_CURRENT_SOURCE_DIR}/source 
	${OGRE_INCLUDE_DIR}
	${BOOST_INCLUDE_DIR})

file(GLOB_RECURSE SOURCE_FILES_CPP ${CMAKE_CURRENT_SOURCE_DIR}/source/*.cpp)
file(GLOB SOURCE_FILES_H ${CMAKE_CURRENT_SOURCE_DIR}/include/*.h)
set(ALL_SOURCE_FILES ${SOURCE_FILES_CPP} ${SOURCE_FILES_H})

if(NOT WIN32)
	set(CMAKE_C_FLAGS ${CMAKE_C_FLAGS} -fPIC)
endif()

add_library(
	${LIB_NAME}
	${LIB_TYPE}
	${ALL_SOURCE_FILES}
)


#SET_TARGET_PROPERTIES (tinyxml PROPERTIES DEFINE_SYMBOL  "EXPORT" )
INSTALL( FILES ${SOURCE_FILES_H} DESTINATION include/ )
install(TARGETS ${LIB_NAME}
  RUNTIME DESTINATION bin/debug CONFIGURATIONS Debug
  LIBRARY DESTINATION bin/debug CONFIGURATIONS Debug
  ARCHIVE DESTINATION lib/debug CONFIGURATIONS Debug
  )

install(TARGETS ${LIB_NAME}
  RUNTIME DESTINATION bin/release CONFIGURATIONS Release
  LIBRARY DESTINATION bin/release CONFIGURATIONS Release
  ARCHIVE DESTINATION lib/release CONFIGURATIONS Release
  )
  
 IF(MSVC)
  INSTALL(DIRECTORY ${PROJECT_BINARY_DIR}/lib/Debug
    DESTINATION lib
    FILES_MATCHING
    PATTERN *.pdb
)
ENDIF(MSVC)
