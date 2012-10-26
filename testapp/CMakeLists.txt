include_directories(../animals)
add_executable(animaltest animaltest.c)
target_link_libraries(animaltest animals)
