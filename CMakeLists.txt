cmake_minimum_required(VERSION 3.15)
project(GameProtoType)

set(CMAKE_CXX_STANDARD 17)

find_package(OpenMP)
if (OPENMP_FOUND)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

find_package(glfw3 3.3 REQUIRED)
find_package(ASSIMP REQUIRED)

include_directories(${ASSIMP_INCLUDE_DIR})

# All dependencies used as source files should be in this folder.
# Note: include_subdirectories is not used as this dependencies folder was originally
# in a different location, so it was easier to just change the location of the folder
# to a local location, as the dependencies folder is now a part of the project folder
# to ease the distribution of this program.
set(DEPENDENCIES_FOLDER ProjectSourceDependencies)

set(GLM_FOLDER ${DEPENDENCIES_FOLDER}/glm-0.9.9.3/glm)
include_directories(${GLM_FOLDER})

# Glad library cannot be included through a "include_directories" command as there are .c files; to prevent undefined
# references to functions, these sources files are added to the 'add_executable; function. These below variables are used
# to make the CMake file easier to change if the location of the glad library changes.
set(GLAD_DIRECTORY ${DEPENDENCIES_FOLDER}/glad)
include_directories(${GLAD_DIRECTORY})

if(UNIX)
    set(CMAKE_PREFIX_PATH /home/binybrion/Qt5.9.9/5.9.9/gcc_64)
    set(GLM_FOLDER /home/binybrion/ProjectSourceDependencies/glm-0.9.9.8/glm)
endif(UNIX)

find_package(Qt5Widgets REQUIRED)
include_directories(${Qt5Widgets_INCLUDES})

set(FAST_NOISE_FOLDER ${DEPENDENCIES_FOLDER}/FastNoise)
include_directories(${FAST_NOISE_FOLDER})

set(POISSON_DISK_FOLDER ${DEPENDENCIES_FOLDER}/PoissonDisk)
include_directories(${POISSON_DISK_FOLDER})

add_executable(GameProtoType main.cpp ${GLAD_DIRECTORY}/glad.c ${FAST_NOISE_FOLDER}/FastNoise.cpp Window/RenderWindow.cpp Window/RenderWindow.h Logging/Logger.cpp
               Logging/Logger.h Logging/LoggerPIMPL.cpp Logging/LoggerPIMPL.h Render/CompileTimeChecks/ValidMappingFlags.h
               Render/CompileTimeChecks/ValidBufferTarget.h Render/CompileTimeChecks/ValidBufferFlags.h
               Render/VBO/BufferStorage.h Render/Shaders/ShaderProgramBase.cpp Render/Shaders/ShaderProgramBase.h
               Render/Shaders/InstanceShaderProgram.cpp Render/Shaders/InstanceShaderProgram.h Render/CommandCentre.cpp
               Render/CommandCentre.h ProgramSettings.h World/BoundingVolumes/AABB.cpp World/BoundingVolumes/AABB.h World/BoundingVolumes/StaticAABB.cpp
               World/RangeImplementation/Range.h World/RangeImplementation/Range.cpp World/BoundingVolumes/StaticAABB.h World/BoundingVolumes/DynamicAABB.cpp
                World/BoundingVolumes/DynamicAABB.h  World/RangeImplementation/Range.cpp World/RangeImplementation/XRange.cpp
                World/RangeImplementation/XRange.h World/RangeImplementation/Range.cpp World/RangeImplementation/Range.h
                World/RangeImplementation/YRange.cpp World/RangeImplementation/YRange.h World/RangeImplementation/ZRange.cpp
                World/RangeImplementation/ZRange.h World/WorldLogic/GridSection.h World/WorldLogic/GridSection.cpp World/WorldLogic/WorldGenerator.cpp
        World/WorldLogic/WorldGenerator.h ProgramInformation/WorldSettings.cpp ProgramInformation/WorldSettings.h Window/Camera/CameraMovement.h
        Window/Camera/CameraObject.cpp Window/Camera/CameraObject.h Render/VAO/TerrainVAO.cpp Render/VAO/TerrainVAO.h Render/VBO/BufferData.h Render/VAO/VAOBase.cpp
        Render/VAO/VAOBase.h ModelLoading/HelperFunctions.cpp ModelLoading/HelperFunctions.h ModelLoading/Mesh.cpp
        ModelLoading/Mesh.h ModelLoading/Model.cpp ModelLoading/Model.h Render/FrustumCulling/FrustumPlane.h Render/FrustumCulling/FrustumCuller.h Render/FrustumCulling/FrustumCuller.cpp
        Render/Textures/CompressFactor.h Render/Textures/TextureBank.cpp Render/Textures/TextureBank.h Render/Textures/TextureID.h Render/Textures/TextureManager.cpp Render/Textures/TextureManager.h
        Render/DataStructures/MeshRenderInformation.h Render/DataStructures/ModelRenderInformation.h Render/DataStructures/RenderStructure.cpp Render/DataStructures/RenderStructure.h Render/DataStructures/GridSectionRenderData.h Render/DataStructures/SortedModel.h Render/DataStructures/GridSectionInstanceRange.h Render/Shaders/SkyBoxShaderProgram.cpp Render/Shaders/SkyBoxShaderProgram.h Render/VAO/SkyBoxVAO.cpp Render/VAO/SkyBoxVAO.h Objects/StaticObject.cpp Objects/StaticObject.h ProgramInformation/AssetLocations.cpp ProgramInformation/AssetLocations.h Render/DataStructures/RenderInformation.h)

target_link_libraries(GameProtoType glfw assimp ${Qt5Widgets_LIBRARIES} -lstdc++fs)