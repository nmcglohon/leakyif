INCLUDE_DIRECTORIES(${ROSS_SOURCE_DIR} ${ROSS_BINARY_DIR})
## Must include binary dir for config.h

## If you will be using the RIO library add this
#INCLUDE_DIRECTORIES(${RIO_SOURCE_DIR} ${RIO_BINARY_DIR})

## Model Version
LIST(APPEND CMAKE_MODULE_PATH "${ROSS_SOURCE_DIR}/cmake/")
INCLUDE(GetGitRevisionDescription)
GET_GIT_HEAD_REVISION(GIT_RESPEC_MODEL GIT_SHA1_MODEL)

## Build Configuration Header File
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_BINARY_DIR})
CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/model_config.h.in ${CMAKE_CURRENT_BINARY_DIR}/model_config.h @ONLY)
configure_file(parseInput.py parseInput.py COPYONLY)
configure_file(graphParserV1.py graphParserV1.py COPYONLY)
configure_file(plotHelper.py plotHelper.py COPYONLY)
configure_file(input.txt input.txt COPYONLY)
SET_SOURCE_FILES_PROPERTIES(${CMAKE_CURRENT_SOURCE_DIR}/model_config.h.in PROPERTIES GENERATED FALSE)
SET_SOURCE_FILES_PROPERTIES(${CMAKE_CURRENT_BINARY_DIR}/model_config.h PROPERTIES GENERATED TRUE)

# All source files for the model
SET(leakyif_srcs
	lif.h
	lif_main.c
	lif_driver.c
	lif_map.c
	iohelper.c
	model_config.h
)

# Set the executable name here: leakyif
# You will be executing the ./leakyif binary
ADD_EXECUTABLE(leakyif ${leakyif_srcs})

TARGET_LINK_LIBRARIES(leakyif ROSS m)
## Alternatively, use this command with RIO
#TARGET_LINK_LIBRARIES(model ROSS RIO m)
