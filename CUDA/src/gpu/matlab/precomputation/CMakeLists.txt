MESSAGE("Generate precomputation MEX file")

set(PRECOMP_MEX_FILE gpuNUFFT_precomp.cpp ${MATLAB_SOURCES})

CUDA_COMPILE(mex_gpuNUFFT_precomp_generated ${PRECOMP_MEX_FILE} SHARED)

CUDA_ADD_LIBRARY(${GRID_MEX_PRECOMP_NAME} SHARED ${mex_gpuNUFFT_precomp_generated} ${PRECOMP_MEX_FILE})
TARGET_LINK_LIBRARIES(${GRID_MEX_PRECOMP_NAME} ${MATLAB_MEX_LIBRARY} ${MATLAB_MX_LIBRARY} ${CUDA_LIBRARIES} ${GRID_LIB_NAME})
SET_TARGET_PROPERTIES(${GRID_MEX_PRECOMP_NAME} PROPERTIES PREFIX "" LINKER_LANGUAGE CXX)

if(WIN32)
  SET_TARGET_PROPERTIES(${GRID_MEX_PRECOMP_NAME} PROPERTIES
						SUFFIX ${MEX_SUFFIX}
						LINK_FLAGS /export:mexFunction)
else(UNIX)
  SET_TARGET_PROPERTIES(${GRID_MEX_PRECOMP_NAME} PROPERTIES SUFFIX ${MEX_SUFFIX})
endif()

exportMex(${GRID_MEX_PRECOMP_NAME})