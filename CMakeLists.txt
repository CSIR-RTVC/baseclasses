cmake_minimum_required(VERSION 3.11)
project(DirectShowBaseClasses VERSION 1.0.0 LANGUAGES CXX)

#Make sure that custom modules like FindRapidJSON are found
list(INSERT CMAKE_MODULE_PATH 0 ${CMAKE_SOURCE_DIR}/cmake)

##############################################
# Declare dependencies

##############################################
# Create target and set properties
add_subdirectory(src)

#Add an alias so that library can be used inside the build tree, e.g. when testing
add_library(BaseClasses::baseclasses ALIAS baseclasses)

# add_subdirectory(test)
