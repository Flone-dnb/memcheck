cmake_minimum_required(VERSION 3.10)

project(memcheck C)

# define this before "add_subdirectory" to this project
#add_link_options(LINKER:--wrap=malloc,--wrap=realloc,--wrap=calloc,--wrap=free,--wrap=strdup,--wrap=wcsdup,--wrap=scandir,--wrap=getline,--wrap=realpath)

add_library(${PROJECT_NAME} SHARED memcheck.h memcheck.c)
target_include_directories(${PROJECT_NAME} PUBLIC .)

# Add pthread
find_package(Threads REQUIRED)
target_link_libraries(${PROJECT_NAME} PRIVATE Threads::Threads)

# Add hashmap
add_library(memcheck_hashmap hashmap.c/hashmap.h hashmap.c/hashmap.c)
target_link_libraries(${PROJECT_NAME} PRIVATE memcheck_hashmap)
