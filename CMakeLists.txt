cmake_minimum_required( VERSION 3.18 )

project( microhttpd )

if( ${CMAKE_BUILD_TYPE} STREQUAL "Debug" )
   set( STATIC_BUILD FALSE CACHE BOOL "" )
else()
   set( STATIC_BUILD TRUE CACHE BOOL "" )
endif()

if( ${STATIC_BUILD} )
   add_library( ${PROJECT_NAME} STATIC )
else()
   add_library( ${PROJECT_NAME} SHARED )
endif()

target_include_directories( ${PROJECT_NAME} PRIVATE
   ${HTTPD_ROOT_PATH}/src/include
   ${HTTPD_ROOT_PATH}/w32/common
)

target_sources( ${PROJECT_NAME} PRIVATE
   ${HTTPD_ROOT_PATH}/src/microhttpd/base64.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/basicauth.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/connection.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/daemon.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/digestauth.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/internal.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/md5.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/memorypool.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/mhd_compat.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/mhd_itc.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/mhd_mono_clock.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/mhd_send.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/mhd_sockets.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/mhd_str.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/mhd_threads.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/postprocessor.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/reason_phrase.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/response.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/sha256.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/sysfdsetsize.c
   ${HTTPD_ROOT_PATH}/src/microhttpd/tsearch.c
)

set_target_properties( ${PROJECT_NAME} PROPERTIES
   CXX_STANDARD 17
)

target_link_libraries( ${PROJECT_NAME} PRIVATE
   Ws2_32
)

target_compile_definitions( ${PROJECT_NAME} PRIVATE
   -DBUILDING_MHD_LIB
   -D_CRT_SECURE_NO_WARNINGS
)

if( ${STATIC_BUILD} )
   target_compile_definitions( ${PROJECT_NAME} PRIVATE
      -D_LIB
      -DMHD_W32LIB
   )
   set_property( TARGET ${PROJECT_NAME} PROPERTY
      MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>"
   )
else()
   target_compile_definitions( ${PROJECT_NAME} PRIVATE
      -D_USRDLL
      -DMHD_W32DLL
      -D_WINDLL
   )
endif()
