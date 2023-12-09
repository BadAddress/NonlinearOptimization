cmake_minimum_required(VERSION 3.2)
project(opt)


set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)



set(OpenGL_GL_PREFERENCE GLVND)



# OpenCV
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})

# Ceres
find_package(Ceres REQUIRED)
include_directories(${CERES_INCLUDE_DIRS})

# g2o
set(CMAKE_PREFIX_PATH "/usr/local/lib/cmake")
find_package(g2o REQUIRED)
include_directories(${G2O_INCLUDE_DIRS})



find_package(CUDAToolkit REQUIRED)

# Eigen
include_directories("/usr/include/eigen3")


add_executable(gaussNewton gaussNewton.cpp)
target_link_libraries(gaussNewton ${OpenCV_LIBS})

add_executable(ceresCurveFitting ceresCurveFitting.cpp)
target_link_libraries(ceresCurveFitting ${OpenCV_LIBS}  ${CERES_LIBRARIES} CUDA::cublas CUDA::cudart CUDA::cusolver CUDA::cusparse)


add_executable(g2oCurveFitting g2oCurveFitting.cpp)
target_link_libraries(g2oCurveFitting ${OpenCV_LIBS} g2o::core g2o::solver_csparse g2o::stuff)
