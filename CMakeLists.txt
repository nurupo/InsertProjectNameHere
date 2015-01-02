cmake_minimum_required(VERSION 2.8)
project(tox)

set(CMAKE_BUILD_TYPE Release)

add_subdirectory(toxcore)
add_subdirectory(toxav)
add_subdirectory(toxdns)
add_subdirectory(toxencryptsave)
