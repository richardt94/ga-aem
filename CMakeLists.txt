cmake_minimum_required(VERSION 3.10)

project(GA_AEM VERSION 2.1
DESCRIPTION "Geoscience Australia AEM Inversion Software"
LANGUAGES CXX)

option(WITH_OMP_GARJMCMC "WITH_OMP_GARJMCMC" OFF)
option(WITH_MPI "WITH_MPI" ON)

#build static libraries for Ross's IO and data types
add_subdirectory(submodules/cpp-utils)

add_executable(garjmcmctdem src/garjmcmctdem.cpp)

set_property(TARGET garjmcmctdem PROPERTY CXX_STANDARD 11)
set_property(TARGET garjmcmctdem PROPERTY CXX_STANDARD_REQUIRED ON)

if(WITH_OMP_GARJMCMC)
  message("Configuring with OpenMP multithreading...")
  find_package(OpenMP)
  if(OpenMP_CXX_FOUND)
    target_link_libraries(garjmcmctdem PUBLIC OpenMP::OpenMP_CXX)
  endif()
endif()

if(WITH_MPI)
  find_package(MPI)

  if(MPI_FOUND)
    message("Configuring with MPI support...")
    target_link_libraries(garjmcmctdem PRIVATE MPI::MPI_CXX)
    target_compile_definitions(garjmcmctdem PRIVATE _MPI_ENABLED)
  else()
    message("Could not find MPI on this system. Configuring without MPI support...")
  endif()
else()
  message("Build without MPI specified. Configuring without MPI support...")
endif()

target_include_directories(garjmcmctdem PRIVATE src)
target_link_libraries(garjmcmctdem PRIVATE general_utils)
target_link_libraries(garjmcmctdem PRIVATE file_utils)
target_link_libraries(garjmcmctdem PRIVATE netcdf netcdf_c++4)
target_link_libraries(garjmcmctdem PRIVATE fftw3)
