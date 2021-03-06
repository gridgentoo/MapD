set(shared_source_files
    Datum.cpp
    timegm.cpp
    mapd_glob.cpp
    StringTransform.cpp
    geo_compression.cpp
    geo_types.cpp
    SysInfo.cpp
    File.cpp
    StackTrace.cpp
    base64.cpp
    Logger.cpp
    misc.cpp
    thread_count.cpp
)
if("${MAPD_EDITION_LOWER}" STREQUAL "ee")
  list(APPEND shared_source_files ee/Encryption.cpp)
endif()

file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/funcannotations.h DESTINATION ${CMAKE_BINARY_DIR}/Shared/)

add_library(Shared ${shared_source_files})
target_link_libraries(Shared ${BLOSC_LIBRARIES} ${Boost_LIBRARIES} ${GDAL_LIBRARIES})
if("${MAPD_EDITION_LOWER}" STREQUAL "ee")
  target_link_libraries(Shared ${OPENSSL_LIBRARIES})
endif()

# Required by ThriftClient.cpp
add_definitions("-DTHRIFT_PACKAGE_VERSION=\"${Thrift_VERSION}\"")
add_library(ThriftClient ThriftClient.cpp)
target_link_libraries(ThriftClient  ${Thrift_LIBRARIES} ${Boost_LIBRARIES})
