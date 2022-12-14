cmake_minimum_required(VERSION 3.1)

project(geowrapper)

set(CMAKE_CXX_FLAGS "-O3")
set(CMAKE_BUILD_TYPE "Release")

# BOOST
#find_package(Boost 1.66 REQUIRED COMPONENTS filesystem locale)

# CGAL
find_package(CGAL REQUIRED QUIET COMPONENTS)
if (CGAL_FOUND)
    if (CGAL_VERSION VERSION_GREATER_EQUAL "5.5")
        message(STATUS "Found CGAL ${CGAL_VERSION}")
    elseif (CGAL_VERSION VERSION_GREATER_EQUAL "5.0")
        message(STATUS "Found CGAL version greater than 5.0, but less than 5.5. "
                "Proceeding to compile with included CGAL headers.")
        include_directories(${CMAKE_SOURCE_DIR}/thirdparty/CGAL/include)
        set(CGAL_USE_INCLUDED_HEADERS ON)
    else()
        message(FATAL_ERROR "Found CGAL version ${CGAL_VERSION} which is not supported!"
                "Please use CGAL version 5")
        return()
    endif ()
endif ()

# Creating entries for target: geowrapper
FILE(GLOB SRC_FILES "*.cpp")
add_executable(geowrapper ${SRC_FILES})
set_target_properties(
        geowrapper
        PROPERTIES CXX_STANDARD 14
)

target_link_libraries(geowrapper
        ${CGAL_LIBRARIES}
        ${CGAL_3RD_PARTY_LIBRARIES}
        )

install(TARGETS geowrapper DESTINATION bin)
