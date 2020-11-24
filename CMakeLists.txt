cmake_minimum_required(VERSION 3.10)

project(GA_AEM VERSION 2.1
DESCRIPTION "Geoscience Australia AEM Inversion Software"
LANGUAGES CXX)

#build static libraries for Ross's IO and data types
add_subdirectory(submodules/cpp-utils)

add_executable(garjmcmctdem src/garjmcmctdem.cpp)

set_property(TARGET garjmcmctdem PROPERTY CXX_STANDARD 11)
set_property(TARGET garjmcmctdem PROPERTY CXX_STANDARD_REQUIRED ON)

target_include_directories(garjmcmctdem PRIVATE src)
target_link_libraries(garjmcmctdem PRIVATE general_utils)
target_link_libraries(garjmcmctdem PRIVATE file_utils)
target_link_libraries(garjmcmctdem PRIVATE netcdf netcdf_c++4)
target_link_libraries(garjmcmctdem PRIVATE fftw3)
