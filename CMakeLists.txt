cmake_minimum_required(VERSION 3.10)
project(crobots)

set(crobots_VERSION_MAJOR 1)
set(crobots_VERSION_MINOR 0)
set(crobots_VERSION "${crobots_VERSION_MAJOR}.${crobots_VERSION_MINOR}")



add_custom_command(
        OUTPUT ../src/tokens.h  ../src/grammar.c
        COMMAND bison --defines=../src/tokens.h -o ../src/grammar.c ../src/grammar.y
)

add_custom_command(
        OUTPUT ../src/lexicon.c
        COMMAND flex -B -o ../src/lexicon.c ../src/lexicon.lex
)


add_library(grammar OBJECT src/grammar.c)
add_library(lexicon OBJECT src/lexicon.c)
add_library(compiler OBJECT src/compiler.c)
add_dependencies(compiler grammar)
add_library(cpu OBJECT src/cpu.c)
add_dependencies(cpu grammar)
add_library(library OBJECT src/library.c)
add_library(display OBJECT src/display.c)
add_library(screen OBJECT src/screen.c)
add_library(motion OBJECT src/motion.c)


add_executable(crobots src/main.c
        $<TARGET_OBJECTS:grammar>
        $<TARGET_OBJECTS:lexicon>
        $<TARGET_OBJECTS:compiler>
        $<TARGET_OBJECTS:cpu>
        $<TARGET_OBJECTS:library>
        $<TARGET_OBJECTS:display>
        $<TARGET_OBJECTS:screen>
        $<TARGET_OBJECTS:motion>
        )

target_link_libraries(crobots m curses)


