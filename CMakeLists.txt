cmake_minimum_required(VERSION 2.8)
project(UnitFormationAlgorithm)

SET(CMAKE_CXX_FLAGS "-std=c++11")

get_filename_component(CDL_MODULES_PATH "./cmake-modules" ABSOLUTE) 
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CDL_MODULES_PATH})

find_package(SFML REQUIRED)

include_directories("src" ${SFML_INCLUDE_DIRS})
file(GLOB UFA_SRC "src/ufa/*.cpp" "src/ufa/entities/*.cpp" "src/ufa/ui/*.cpp" "src/ufa/logic/*.cpp" "src/ufa/general/*.cpp")

add_executable(${PROJECT_NAME} ${UFA_SRC})
target_link_libraries(${PROJECT_NAME} ${SFML_LIBRARIES})

foreach(SFML_DYN_LIB ${SFML_DYNAMIC_LIBS})
	add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
		COMMAND ${CMAKE_COMMAND} -E copy_if_different
			${SFML_DYN_LIB}
			$<TARGET_FILE_DIR:${PROJECT_NAME}>)
endforeach(SFML_DYN_LIB ${SFML_DYNAMIC_LIBS})
