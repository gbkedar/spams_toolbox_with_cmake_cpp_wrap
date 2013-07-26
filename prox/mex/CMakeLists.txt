SET( FISTA_FLAT_SRCS mexFistaFlat.cpp )

INCLUDE_DIRECTORIES( ${Spams-toolbox_SOURCE_DIR}/prox ${Spams-toolbox_SOURCE_DIR}/linalg )

ADD_LIBRARY( FISTA_FLAT ${FISTA_FLAT_SRCS} )
