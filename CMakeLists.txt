cmake_minimum_required(VERSION 3.1)
set(CMAKE_BUILD_TYPE Release)
set(CMAKE_CXX_STANDARD 17)
project (µWebSockets)

find_package(OpenSSL REQUIRED)

link_directories(${CMAKE_INSTALL_PREFIX}/lib)

set(LIBUV_INCLUDE_DIR ../libuv/include)
set (LIBUV_LIBRARY libuv.so)

add_library(uWS SHARED 
    fuzzing/WebSocket.cpp)
target_include_directories(uWS PUBLIC src)


target_include_directories(uWS PUBLIC ${LIBUV_INCLUDE_DIR})

target_include_directories(uWS PUBLIC ${OPENSSL_INCLUDE_DIR})
target_link_libraries (uWS LINK_PUBLIC ${LIBUV_LIBRARY})
target_link_libraries (uWS LINK_PUBLIC ${OPENSSL_SSL_LIBRARY})
target_link_libraries (uWS LINK_PUBLIC ${OPENSSL_CRYPTO_LIBRARY})

if (UNIX)
find_package(ZLIB REQUIRED)
target_include_directories(uWS PUBLIC ${ZLIB_INCLUDE_DIRS})
target_link_libraries (uWS LINK_PUBLIC ${ZLIB_LIBRARY})
endif (UNIX)


target_link_libraries (uWS LINK_PUBLIC pthread)
install (TARGETS uWS DESTINATION ${CMAKE_INSTALL_PREFIX}/lib)
install (DIRECTORY src/ DESTINATION ${CMAKE_INSTALL_PREFIX}/include/uWS PATTERN "src/*.h" )
