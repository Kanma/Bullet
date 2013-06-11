cmake_minimum_required(VERSION 2.6)
project(BULLET)


##########################################################################################
# Settings

if (NOT DEFINED USE_DOUBLE_PRECISION)
    set(BULLET_USE_DOUBLE_PRECISION OFF CACHE BOOL "Use double precision")
else()
    set(BULLET_USE_DOUBLE_PRECISION ${USE_DOUBLE_PRECISION})
endif()

if (NOT DEFINED BULLET_NO_PROFILE)
    set(BULLET_NO_PROFILE OFF CACHE BOOL "Disable the profiling feature")
endif()


##########################################################################################
# XMake-related settings

# Set the output paths
if (NOT DEFINED XMAKE_BINARY_DIR)
    set(XMAKE_BINARY_DIR "${BULLET_BINARY_DIR}")
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${XMAKE_BINARY_DIR}/bin")
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${XMAKE_BINARY_DIR}/lib")
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${XMAKE_BINARY_DIR}/bin")
endif()

# Set the dependencies path
if (NOT DEFINED XMAKE_DEPENDENCIES_DIR)
    set(XMAKE_DEPENDENCIES_DIR "${BULLET_SOURCE_DIR}/dependencies")
endif()


##########################################################################################
# XMake importation

if (NOT EXISTS "${XMAKE_DEPENDENCIES_DIR}/XMake/XMake.cmake")
    message(FATAL_ERROR
"Missing dependency: XMake
This distribution of Bullet requires XMake.

It is provided as a GIT submodule of this repository.

Did you forgot to execute the following commands?
   git submodule init
   git submodule update")
endif()

include("${XMAKE_DEPENDENCIES_DIR}/XMake/XMake.cmake")


##########################################################################################
# Declaration of the library

# List the source files
set(BulletCollision_SRCS
    BulletCollision/BroadphaseCollision/btAxisSweep3.cpp
    BulletCollision/BroadphaseCollision/btCollisionAlgorithm.cpp
    BulletCollision/BroadphaseCollision/btDbvt.cpp
    BulletCollision/BroadphaseCollision/btDbvtBroadphase.cpp
    BulletCollision/BroadphaseCollision/btDispatcher.cpp
    BulletCollision/BroadphaseCollision/btMultiSapBroadphase.cpp
    BulletCollision/BroadphaseCollision/btOverlappingPairCache.cpp
    BulletCollision/BroadphaseCollision/btQuantizedBvh.cpp
    BulletCollision/BroadphaseCollision/btSimpleBroadphase.cpp
    BulletCollision/CollisionDispatch/btActivatingCollisionAlgorithm.cpp
    BulletCollision/CollisionDispatch/btBoxBoxCollisionAlgorithm.cpp
    BulletCollision/CollisionDispatch/btBox2dBox2dCollisionAlgorithm.cpp
    BulletCollision/CollisionDispatch/btBoxBoxDetector.cpp
    BulletCollision/CollisionDispatch/btCollisionDispatcher.cpp
    BulletCollision/CollisionDispatch/btCollisionObject.cpp
    BulletCollision/CollisionDispatch/btCollisionWorld.cpp
    BulletCollision/CollisionDispatch/btCompoundCollisionAlgorithm.cpp
    BulletCollision/CollisionDispatch/btConvexConcaveCollisionAlgorithm.cpp
    BulletCollision/CollisionDispatch/btConvexConvexAlgorithm.cpp
    BulletCollision/CollisionDispatch/btConvexPlaneCollisionAlgorithm.cpp
    BulletCollision/CollisionDispatch/btConvex2dConvex2dAlgorithm.cpp
    BulletCollision/CollisionDispatch/btDefaultCollisionConfiguration.cpp
    BulletCollision/CollisionDispatch/btEmptyCollisionAlgorithm.cpp
    BulletCollision/CollisionDispatch/btGhostObject.cpp
    BulletCollision/CollisionDispatch/btInternalEdgeUtility.cpp
    BulletCollision/CollisionDispatch/btInternalEdgeUtility.h
    BulletCollision/CollisionDispatch/btManifoldResult.cpp
    BulletCollision/CollisionDispatch/btSimulationIslandManager.cpp
    BulletCollision/CollisionDispatch/btSphereBoxCollisionAlgorithm.cpp
    BulletCollision/CollisionDispatch/btSphereSphereCollisionAlgorithm.cpp
    BulletCollision/CollisionDispatch/btSphereTriangleCollisionAlgorithm.cpp
    BulletCollision/CollisionDispatch/btUnionFind.cpp
    BulletCollision/CollisionDispatch/SphereTriangleDetector.cpp
    BulletCollision/CollisionShapes/btBoxShape.cpp
    BulletCollision/CollisionShapes/btBox2dShape.cpp
    BulletCollision/CollisionShapes/btBvhTriangleMeshShape.cpp
    BulletCollision/CollisionShapes/btCapsuleShape.cpp
    BulletCollision/CollisionShapes/btCollisionShape.cpp
    BulletCollision/CollisionShapes/btCompoundShape.cpp
    BulletCollision/CollisionShapes/btConcaveShape.cpp
    BulletCollision/CollisionShapes/btConeShape.cpp
    BulletCollision/CollisionShapes/btConvexHullShape.cpp
    BulletCollision/CollisionShapes/btConvexInternalShape.cpp
    BulletCollision/CollisionShapes/btConvexPointCloudShape.cpp
    BulletCollision/CollisionShapes/btConvexPolyhedron.cpp
    BulletCollision/CollisionShapes/btConvexShape.cpp
    BulletCollision/CollisionShapes/btConvex2dShape.cpp
    BulletCollision/CollisionShapes/btConvexTriangleMeshShape.cpp
    BulletCollision/CollisionShapes/btCylinderShape.cpp
    BulletCollision/CollisionShapes/btEmptyShape.cpp
    BulletCollision/CollisionShapes/btHeightfieldTerrainShape.cpp
    BulletCollision/CollisionShapes/btMinkowskiSumShape.cpp
    BulletCollision/CollisionShapes/btMultimaterialTriangleMeshShape.cpp
    BulletCollision/CollisionShapes/btMultiSphereShape.cpp
    BulletCollision/CollisionShapes/btOptimizedBvh.cpp
    BulletCollision/CollisionShapes/btPolyhedralConvexShape.cpp
    BulletCollision/CollisionShapes/btScaledBvhTriangleMeshShape.cpp
    BulletCollision/CollisionShapes/btShapeHull.cpp
    BulletCollision/CollisionShapes/btSphereShape.cpp
    BulletCollision/CollisionShapes/btStaticPlaneShape.cpp
    BulletCollision/CollisionShapes/btStridingMeshInterface.cpp
    BulletCollision/CollisionShapes/btTetrahedronShape.cpp
    BulletCollision/CollisionShapes/btTriangleBuffer.cpp
    BulletCollision/CollisionShapes/btTriangleCallback.cpp
    BulletCollision/CollisionShapes/btTriangleIndexVertexArray.cpp
    BulletCollision/CollisionShapes/btTriangleIndexVertexMaterialArray.cpp
    BulletCollision/CollisionShapes/btTriangleMesh.cpp
    BulletCollision/CollisionShapes/btTriangleMeshShape.cpp
    BulletCollision/CollisionShapes/btUniformScalingShape.cpp
    BulletCollision/Gimpact/btContactProcessing.cpp
    BulletCollision/Gimpact/btGenericPoolAllocator.cpp
    BulletCollision/Gimpact/btGImpactBvh.cpp
    BulletCollision/Gimpact/btGImpactCollisionAlgorithm.cpp
    BulletCollision/Gimpact/btGImpactQuantizedBvh.cpp
    BulletCollision/Gimpact/btGImpactShape.cpp
    BulletCollision/Gimpact/btTriangleShapeEx.cpp
    BulletCollision/Gimpact/gim_box_set.cpp
    BulletCollision/Gimpact/gim_contact.cpp
    BulletCollision/Gimpact/gim_memory.cpp
    BulletCollision/Gimpact/gim_tri_collision.cpp
    BulletCollision/NarrowPhaseCollision/btContinuousConvexCollision.cpp
    BulletCollision/NarrowPhaseCollision/btConvexCast.cpp
    BulletCollision/NarrowPhaseCollision/btGjkConvexCast.cpp
    BulletCollision/NarrowPhaseCollision/btGjkEpa2.cpp
    BulletCollision/NarrowPhaseCollision/btGjkEpaPenetrationDepthSolver.cpp
    BulletCollision/NarrowPhaseCollision/btGjkPairDetector.cpp
    BulletCollision/NarrowPhaseCollision/btMinkowskiPenetrationDepthSolver.cpp
    BulletCollision/NarrowPhaseCollision/btPersistentManifold.cpp
    BulletCollision/NarrowPhaseCollision/btPolyhedralContactClipping.cpp
    BulletCollision/NarrowPhaseCollision/btRaycastCallback.cpp
    BulletCollision/NarrowPhaseCollision/btSubSimplexConvexCast.cpp
    BulletCollision/NarrowPhaseCollision/btVoronoiSimplexSolver.cpp
)


set(BulletDynamics_SRCS
    BulletDynamics/Character/btKinematicCharacterController.cpp
    BulletDynamics/ConstraintSolver/btConeTwistConstraint.cpp
    BulletDynamics/ConstraintSolver/btContactConstraint.cpp
    BulletDynamics/ConstraintSolver/btGearConstraint.cpp
    BulletDynamics/ConstraintSolver/btGeneric6DofConstraint.cpp
    BulletDynamics/ConstraintSolver/btGeneric6DofSpringConstraint.cpp
    BulletDynamics/ConstraintSolver/btHinge2Constraint.cpp
    BulletDynamics/ConstraintSolver/btHingeConstraint.cpp
    BulletDynamics/ConstraintSolver/btPoint2PointConstraint.cpp
    BulletDynamics/ConstraintSolver/btSequentialImpulseConstraintSolver.cpp
    BulletDynamics/ConstraintSolver/btSliderConstraint.cpp
    BulletDynamics/ConstraintSolver/btSolve2LinearConstraint.cpp
    BulletDynamics/ConstraintSolver/btTypedConstraint.cpp
    BulletDynamics/ConstraintSolver/btUniversalConstraint.cpp
    BulletDynamics/Dynamics/btDiscreteDynamicsWorld.cpp
    BulletDynamics/Dynamics/btRigidBody.cpp
    BulletDynamics/Dynamics/btSimpleDynamicsWorld.cpp
    BulletDynamics/Vehicle/btRaycastVehicle.cpp
    BulletDynamics/Vehicle/btWheelInfo.cpp
)


set(BulletSoftBody_SRCS
    BulletSoftBody/btDefaultSoftBodySolver.cpp
    BulletSoftBody/btSoftBody.cpp
    BulletSoftBody/btSoftBodyConcaveCollisionAlgorithm.cpp
    BulletSoftBody/btSoftBodyHelpers.cpp
    BulletSoftBody/btSoftBodyRigidBodyCollisionConfiguration.cpp
    BulletSoftBody/btSoftRigidCollisionAlgorithm.cpp
    BulletSoftBody/btSoftRigidDynamicsWorld.cpp
    BulletSoftBody/btSoftSoftCollisionAlgorithm.cpp
)


set(LinearMath_SRCS
    LinearMath/btAlignedAllocator.cpp
    LinearMath/btConvexHull.cpp
    LinearMath/btConvexHullComputer.cpp
    LinearMath/btGeometryUtil.cpp
    LinearMath/btPolarDecomposition.cpp
    LinearMath/btQuickprof.cpp
    LinearMath/btSerializer.cpp
    LinearMath/btVector3.cpp
)


set(SRCS ${BulletCollision_SRCS}
         ${BulletDynamics_SRCS}
         ${BulletSoftBody_SRCS}
         ${LinearMath_SRCS}
)


# List the include paths
set(INCLUDE_PATHS "${BULLET_SOURCE_DIR}"
                  "${XMAKE_BINARY_DIR}/include")

include_directories(${INCLUDE_PATHS})


# Generate the configuration file
set(BT_USE_DOUBLE_PRECISION ${BULLET_USE_DOUBLE_PRECISION})
set(BT_NO_PROFILE ${BULLET_NO_PROFILE})

configure_file(${BULLET_SOURCE_DIR}/btConfig.h.in
               ${XMAKE_BINARY_DIR}/include/Bullet/btConfig.h)


if (BT_USE_DOUBLE_PRECISION)
    add_definitions(-DBT_USE_DOUBLE_PRECISION)
endif()

if (BT_NO_PROFILE)
    add_definitions(-DBT_NO_PROFILE)
endif()


# Declaration of the library
xmake_create_static_library(BULLET bullet ${SRCS})


# Exportations
xmake_export_include_paths(BULLET ${INCLUDE_PATHS})
