set(TARGET toxcore)

aux_source_directory(. SRC_LIST)

find_package(Threads)
include_directories(../prefix/include)

add_library(${TARGET} STATIC ${SRC_LIST})
target_link_libraries(${TARGET} ${CMAKE_THREAD_LIBS_INIT} ../prefix/lib/libsodium.a)

