set(TARGET toxencryptsave)

aux_source_directory(. SRC_LIST)

include_directories(../prefix/include)

add_library(${TARGET} ${SRC_LIST})
target_link_libraries(${TARGET})

