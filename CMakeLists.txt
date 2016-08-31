PROJECT(GIT_MODULE)
CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

OPTION(BUILD_CLAR "Build Tests using the Clar suite" ON)

FIND_PATH(LIBGIT2_INCLUDE_DIR NAMES git2.h)
FIND_LIBRARY(LIBGIT2_LIBRARIES NAMES libgit2.so)

SET(SRC_ALL common.c
            hash_object.c
            pack_objects.c
            unpack_objects.c)

ADD_EXECUTABLE(git_module git_module.c ${SRC_ALL})

IF(LIBGIT2_LIBRARIES)
    MESSAGE("LIBGIT2")
    TARGET_LINK_LIBRARIES(git_module ${LIBGIT2_LIBRARIES})
ELSE()
    MESSAGE("LIBGIT2 library is not found")
ENDIF()

IF(BUILD_CLAR)
    MESSAGE("BUILD_CLAR is ON")
    SET(TEST_DIR tests)
    SET(TEST_ALL ${TEST_DIR}/hash_object.c
                 ${TEST_DIR}/clar.c
                 ${TEST_DIR}/main.c)
    ADD_EXECUTABLE(git_module_test ${SRC_ALL} ${TEST_ALL})
    IF(LIBGIT2_LIBRARIES)
        TARGET_LINK_LIBRARIES(git_module_test ${LIBGIT2_LIBRARIES})
    ENDIF()
ENDIF()
