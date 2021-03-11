# CMake entry point
cmake_minimum_required (VERSION 3.0)
project (Tutorials)

find_package(OpenGL REQUIRED)


if( CMAKE_BINARY_DIR STREQUAL CMAKE_SOURCE_DIR )
    message( FATAL_ERROR "Please select another Build Directory ! (and give it a clever name, like bin_Visual2012_64bits/)" )
endif()
if( CMAKE_SOURCE_DIR MATCHES " " )
	message( "Your Source Directory contains spaces. If you experience problems when compiling, this can be the cause." )
endif()
if( CMAKE_BINARY_DIR MATCHES " " )
	message( "Your Build Directory contains spaces. If you experience problems when compiling, this can be the cause." )
endif()



# Compile external dependencies 
add_subdirectory (external)

# On Visual 2005 and above, this module can set the debug working directory
cmake_policy(SET CMP0026 OLD)
list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/external/rpavlik-cmake-modules-fe2273")
include(CreateLaunchers)
include(MSVCMultipleProcessCompile) # /MP

if(INCLUDE_DISTRIB)
	add_subdirectory(distrib)
endif(INCLUDE_DISTRIB)



include_directories(
	external/AntTweakBar-1.16/include/
	external/glfw-3.1.2/include/
	external/glm-0.9.7.1/
	external/glew-1.13.0/include/
	external/assimp-3.0.1270/include/
	external/bullet-2.81-rev2613/src/
	.
)

set(ALL_LIBS
	${OPENGL_LIBRARY}
	glfw
	GLEW_1130
)

add_definitions(
	-DTW_STATIC
	-DTW_NO_LIB_PRAGMA
	-DTW_NO_DIRECT3D
	-DGLEW_STATIC
	-D_CRT_SECURE_NO_WARNINGS
)

# Task 1
add_executable(task1 
	task1/task1.cpp
	common/shader.cpp
	common/shader.hpp
	
	task1/SimpleFragmentShader.fragmentshader
	task1/SimpleVertexShader.vertexshader
)
target_link_libraries(task1
	${ALL_LIBS}
)

# Xcode and Visual working directories
set_target_properties(task1 PROPERTIES XCODE_ATTRIBUTE_CONFIGURATION_BUILD_DIR "${CMAKE_CURRENT_SOURCE_DIR}/task1/")
create_target_launcher(task1 WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/task1/")
create_default_target_launcher(task1 WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/task1/") 

# Task 2
add_executable(task2 
	task2/task2.cpp
	common/shader.cpp
	common/shader.hpp
	
	task2/VertexShader.glsl
	task2/RedFragmentShader.glsl
	task2/GreenFragmentShader.glsl
)
target_link_libraries(task2
	${ALL_LIBS}
)

# Xcode and Visual working directories
set_target_properties(task2 PROPERTIES XCODE_ATTRIBUTE_CONFIGURATION_BUILD_DIR "${CMAKE_CURRENT_SOURCE_DIR}/task2/")
create_target_launcher(task2 WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/task2/")
create_default_target_launcher(task2 WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/task2/") 

# Task 3
add_executable(task3 
	task3/task3.cpp
	common/shader.cpp
	common/shader.hpp
	
	task3/SimpleTransform.glsl
	task2/RedFragmentShader.glsl
	task2/GreenFragmentShader.glsl
)
target_link_libraries(task3
	${ALL_LIBS}
)

# Xcode and Visual working directories
set_target_properties(task3 PROPERTIES XCODE_ATTRIBUTE_CONFIGURATION_BUILD_DIR "${CMAKE_CURRENT_SOURCE_DIR}/task3/")
create_target_launcher(task3 WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/task3/")
create_default_target_launcher(task2 WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/task3/") 

# Task 4
add_executable(task4 
	task4/task4.cpp
	common/shader.cpp
	common/shader.hpp
)
target_link_libraries(task4
	${ALL_LIBS}
)
# Xcode and Visual working directories
set_target_properties(task4 PROPERTIES XCODE_ATTRIBUTE_CONFIGURATION_BUILD_DIR "${CMAKE_CURRENT_SOURCE_DIR}/task4/")
create_target_launcher(task4 WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/task4/")
create_default_target_launcher(task4 WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/task4/") 

# SOURCE_GROUP(common REGULAR_EXPRESSION ".*/common/.*" )
# SOURCE_GROUP(shaders REGULAR_EXPRESSION ".*/.*shader$" )


# if (NOT ${CMAKE_GENERATOR} MATCHES "Xcode" )
# add_custom_command(
#    TARGET task3 POST_BUILD
#    COMMAND ${CMAKE_COMMAND} -E copy "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_CFG_INTDIR}/task3${CMAKE_EXECUTABLE_SUFFIX}" "${CMAKE_CURRENT_SOURCE_DIR}/task3/"
# )
# elseif (${CMAKE_GENERATOR} MATCHES "Xcode" )

# endif (NOT ${CMAKE_GENERATOR} MATCHES "Xcode" )

