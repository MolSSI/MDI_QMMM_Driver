# Locate MPI

find_package(MPI)
if(MPI_FOUND)
   include_directories(${MPI_INCLUDE_PATH})
else()
   configure_file(${CMAKE_CURRENT_SOURCE_DIR}/../STUBS_MPI/mpi.h ${CMAKE_CURRENT_BINARY_DIR}/../STUBS_MPI/mpi.h COPYONLY)
endif()



# Link to MDI

link_directories( ${mdi_lib_path} )
include_directories( ${mdi_include_path} )



# Compile the engine

add_executable(engine_cxx
               engine_cxx.cpp)
target_link_libraries(engine_cxx mdi
                      ${MPI_LIBRARIES})
set_target_properties(engine_cxx PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}")



# Ensure that MPI is properly linked

if(NOT MPI_FOUND)
   target_include_directories(engine_cxx PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/../STUBS_MPI/)
endif()
if(MPI_COMPILE_FLAGS)
   set_target_properties(engine_cxx PROPERTIES
      COMPILE_FLAGS "${MPI_COMPILE_FLAGS}")
endif()
if(MPI_LINK_FLAGS)
   set_target_properties(engine_cxx PROPERTIES
      LINK_FLAGS "${MPI_LINK_FLAGS}")
endif()
