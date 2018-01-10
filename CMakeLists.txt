cmake_minimum_required(VERSION 2.8)
SET(PROJECT_NAME DisplayImage)
project( ${PROJECT_NAME} )
FILE(GLOB_RECURSE SRCS src/*.cpp)

IF(WIN32)
  MESSAGE("This is WIN32")
  SET(OPENCV_LIBS opencv_world330.lib)
  SET(CMAKE_GENERATOR_PLATFORM x64)

  IF (DEFINED ENV{OPENCV})
     set(OPENCV_BUILD $ENV{OPENCV} CACHE FILEPATH "PATH for OPENCV BUILD")

  ELSE()
      FIND_PATH(OPENCV_BUILD_GUESS build PATHS 3rdparty/opencv)
      IF(NOT OPENCV_BUILD_GUESS)
        set(OPENCV_BUILD NOTFOUND CACHE FILEPATH "PATH for OPENCV BUILD")
      ELSE()
        set(OPENCV_BUILD ${OPENCV_BUILD_GUESS}/build)
      ENDIF()
  ENDIF()

  IF(OPENCV_BUILD)
      MESSAGE("OPENCV: ${OPENCV_BUILD}")
      set(OPENCV_INCLUDE ${OPENCV_BUILD}/include)
      set(OPENCV_LIB ${OPENCV_BUILD}/x64/vc14/lib)
      include_directories(${OPENCV_INCLUDE})
      link_directories(${OPENCV_LIB})
  ELSE()
      MESSAGE(FATAL_ERROR "OPENCV NOT FOUND! Fix the path")
  ENDIF()

 # All
 add_executable(${PROJECT_NAME} ${SRCS})
 target_link_libraries(${PROJECT_NAME} ${OPENCV_LIBS})


ELSE() # Linux & Mac
  MESSAGE("This is Linux or Mac")
  find_package( OpenCV REQUIRED )
  include_directories( ${OpenCV_INCLUDE_DIRS} )
  add_executable( ${PROJECT_NAME} ${SRCS})
  target_link_libraries( ${PROJECT_NAME} ${OpenCV_LIBS})

ENDIF()
