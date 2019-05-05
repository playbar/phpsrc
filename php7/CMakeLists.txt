cmake_minimum_required(VERSION 3.4.1)
project(php7 LANGUAGES C)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14 -fexceptions -Wno-unused-result -Wsign-compare \
 -DNDEBUG -g -fwrapv -O3 -Wall -Wextra -Wno-unused-result -Wno-unused-parameter
  -Wno-missing-field-initializers -Werror=implicit-function-declaration ")

add_definitions(-DZEND_ENABLE_STATIC_TSRMLS_CACHE=1)
add_definitions(-DZEND_SIGNALS )

set(py_parser_src
        Parser/acceler.c
        Parser/bitset.c
        Parser/firstsets.c
        Parser/grammar.c
        Parser/grammar1.c
        Parser/listnode.c
        Parser/metagrammar.c
        Parser/myreadline.c
        Parser/node.c
        Parser/parser.c
        Parser/parsetok.c
        Parser/pgen.c
        Parser/tokenizer.c
        )


add_library( phplib STATIC
        ${py_parser_src}
       )

target_include_directories(phplib PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}/include)

#

set(php_main
       Programs/python.c)
add_executable(php ${php_main})
target_include_directories(php PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_link_libraries(php phplib)


