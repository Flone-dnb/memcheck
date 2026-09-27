cmake_minimum_required(VERSION 3.10)

project(memcheck C)

# on Linux define this before "add_subdirectory" to this project
#add_link_options(LINKER:--wrap=malloc,--wrap=realloc,--wrap=calloc,--wrap=free,--wrap=strdup,--wrap=wcsdup,--wrap=scandir,--wrap=getline,--wrap=realpath)

set(LIBRARY_TYPE SHARED)
if(MSVC)
	set(LIBRARY_TYPE STATIC)
endif()

add_library(${PROJECT_NAME} ${LIBRARY_TYPE} memcheck.h memcheck.c)
target_include_directories(${PROJECT_NAME} PUBLIC .)

if(NOT MSVC)
	# Add pthread
	find_package(Threads REQUIRED)
	target_link_libraries(${PROJECT_NAME} PRIVATE Threads::Threads)
	
	# Add hashmap
	add_library(memcheck_hashmap hashmap.c/hashmap.h hashmap.c/hashmap.c)
	target_link_libraries(${PROJECT_NAME} PRIVATE memcheck_hashmap)
endif()