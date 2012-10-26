add_library(animals SHARED cat.c dog.c)
set_target_properties(
    animals
    PROPERTIES
    DEBUG_POSTFIX "_d"
    SOVERSION ${VERSION_MAJOR_MINOR}
    VERSION ${VERSION_MAJOR_MINOR_PATCH_BUILD}
)
