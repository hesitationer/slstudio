include_directories(${CMAKE_SOURCE_DIR} ${CMAKE_BINARY_DIR}
        ${CMAKE_SOURCE_DIR}/src ${CMAKE_SOURCE_DIR}/src/camera)



if(WITH_CAMERAXIMEA)
    include("${CMAKE_SOURCE_DIR}/cmake/FindXimea.cmake")
    if(XIMEA_FOUND)
        if(APPLE)
            FIND_LIBRARY(XIMEA_LIBRARY NAMES m3api)
            FIND_PATH(XIMEA_PATH xiApi.h)
        else()
            set(XIMEA_LIBRARY xiapi64)
        endif()
        include_directories(${XIMEA_PATH})
        add_library(XimeaCamera STATIC CameraXIMEA.cpp CameraXIMEA.h)
        target_link_libraries(XimeaCamera ${XIMEA_LIBRARY})
		set(DEVICE_LIBS ${DEVICE_LIBS} XimeaCamera ${XIMEA_LIBRARY} PARENT_SCOPE)
    else(XIMEA_FOUND)
        message(SEND_ERROR "Could not find Ximea camera support.")
    endif(XIMEA_FOUND)
endif(WITH_CAMERAXIMEA)
