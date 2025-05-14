cmake_minimum_required(VERSION 3.30)
project(2DGameEngineCmake2)

set(CMAKE_CXX_STANDARD 20)
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} --static -static-libstdc++ -static-libgcc")


file(GLOB_RECURSE GAME_ENGINE_SOURCES CONFIGURE_DEPENDS
        ${CMAKE_CURRENT_SOURCE_DIR}/GameEngine/*.cpp
)

add_executable(2DGameEngineCmake2 main.cpp
${GAME_ENGINE_SOURCES}
)

find_package(SDL2 CONFIG REQUIRED)

find_package(SDL2_Image CONFIG REQUIRED)

find_package(SDL2_ttf CONFIG REQUIRED)

find_package(Lua CONFIG REQUIRED)

find_package(glm CONFIG REQUIRED)

find_package(sol2 CONFIG REQUIRED)


# set the include directory
target_include_directories(
        2DGameEngineCmake2
        PUBLIC
        ${SDL2_INCLUDE_DIRS}
        ${SDL2_IMAGE_INCLUDE_DIRS}
        ${SDL2_TTF_INCLUDE_DIRS}
        ${LUA_INCLUDE_DIRS}
        ${GLM_INCLUDE_DIRS}
        ${SOL2_INCLUDE_DIRS}
)

# link all libraries to the project
target_link_libraries(
        2DGameEngineCmake2
        PRIVATE
        SDL2::SDL2
        SDL2::SDL2main
        SDL2_image::SDL2_image
        SDL2_ttf::SDL2_ttf
        ${LUA_LIBRARIES}
        glm::glm
        ${SOL2_LIBRARIES}
)

function (copy_dlls target)
    if (WIN32)
        add_custom_command (
                TARGET "${target}" POST_BUILD
                COMMAND "${CMAKE_COMMAND}" -E copy -t "$<TARGET_FILE_DIR:${target}>"
                "$<TARGET_RUNTIME_DLLS:${target}>" USES_TERMINAL COMMAND_EXPAND_LISTS
        )
    endif ()
endfunction ()

copy_dlls(2DGameEngineCmake2)

#if (WIN64)
#    add_custom_command(
#            TARGET 2DGameEngineCmake2 POST_BUILD
#            COMMAND "${CMAKE_COMMAND}" -E copy_if_different "$<TARGET_FILE:SDL2::SDL2>" "$<TARGET_FILE_DIR:2DGameEngineCmake2>"
#            COMMAND "${CMAKE_COMMAND}" -E copy_if_different "$<TARGET_FILE:SDL2_image::SDL2_image>" "$<TARGET_FILE_DIR:2DGameEngineCmake2>"
#            COMMAND "${CMAKE_COMMAND}" -E copy_if_different "$<TARGET_FILE:SDL2_ttf::SDL2_ttf>" "$<TARGET_FILE_DIR:2DGameEngineCmake2>"
#            VERBATIM
#    )
#endif()









