project(tokendb_library C)

set(TOKENDB_PUBLIC_INCLUDE_DIRS
  ${CMAKE_CURRENT_BINARY_DIR}
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${TPS_INCLUDE_DIR}
  CACHE INTERNAL "tokendb public include directories"
)

set(TOKENDB_PRIVATE_INCLUDE_DIRS
  ${TOKENDB_PUBLIC_INCLUDE_DIRS}
  ${CMAKE_BINARY_DIR}
  ${NSPR_INCLUDE_DIRS}
  ${NSS_INCLUDE_DIRS}
  ${LDAP_INCLUDE_DIRS}
)

set(TOKENDB_SHARED_LIBRARY
  tokendb_library
  CACHE INTERNAL "tokendb shared library"
)

set(TOKENDB_LINK_LIBRARIES
  ${NSPR_LIBRARIES}
  ${NSS_LIBRARIES}
  ${LDAP_LIBRARIES}
)

set(tokendb_library_SRCS
    tus_db.c
)

include_directories(${TOKENDB_PRIVATE_INCLUDE_DIRS})

add_library(${TOKENDB_SHARED_LIBRARY} SHARED ${tokendb_library_SRCS})
add_dependencies(${TOKENDB_SHARED_LIBRARY} pki-tps-jar)
target_link_libraries(${TOKENDB_SHARED_LIBRARY} ${TOKENDB_LINK_LIBRARIES})

set_target_properties(${TOKENDB_SHARED_LIBRARY}
    PROPERTIES
        OUTPUT_NAME
            tokendb
)

install(
    TARGETS
        ${TOKENDB_SHARED_LIBRARY}
    LIBRARY DESTINATION ${LIB_INSTALL_DIR}/tps
)
