cmake_minimum_required(VERSION 2.6)
project(BULLET)

# Options
if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the type of build, options are: None (CMAKE_CXX_FLAGS or CMAKE_C_FLAGS used) Debug Release RelWithDebInfo MinSizeRel." FORCE)
endif()

IF (USE_DOUBLE_PRECISION)
    ADD_DEFINITIONS( -DBT_USE_DOUBLE_PRECISION)
ENDIF (USE_DOUBLE_PRECISION)


# Set the output path
if (NOT ATHENA_BINARY_DIR)
    set(ATHENA_BINARY_DIR "${BULLET_BINARY_DIR}")
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${BULLET_BINARY_DIR}/lib")
endif()


# XMake settings
if (EXISTS "${BULLET_SOURCE_DIR}/dependencies/xmake/XMake.cmake")
    include("${BULLET_SOURCE_DIR}/dependencies/xmake/XMake.cmake")
endif()

xmake_init_static(BULLET)
xmake_add_include_paths(BULLET ${BULLET_SOURCE_DIR})
xmake_add_link_paths(BULLET "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY}")
xmake_add_link_targets(BULLET bullet)

include_directories(${BULLET_SOURCE_DIR})


# List the source files
SET(BulletCollision_SRCS
	BulletCollision/BroadphaseCollision/btAxisSweep3.cpp
    # BulletCollision/BroadphaseCollision/btBroadphaseProxy.cpp
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
	BulletCollision/NarrowPhaseCollision/btRaycastCallback.cpp
	BulletCollision/NarrowPhaseCollision/btSubSimplexConvexCast.cpp
	BulletCollision/NarrowPhaseCollision/btVoronoiSimplexSolver.cpp
)


SET(BulletDynamics_SRCS
	BulletDynamics/Character/btKinematicCharacterController.cpp
	BulletDynamics/ConstraintSolver/btConeTwistConstraint.cpp
	BulletDynamics/ConstraintSolver/btContactConstraint.cpp
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
	BulletDynamics/Dynamics/btContinuousDynamicsWorld.cpp
	BulletDynamics/Dynamics/btDiscreteDynamicsWorld.cpp
	BulletDynamics/Dynamics/btRigidBody.cpp
	BulletDynamics/Dynamics/btSimpleDynamicsWorld.cpp
	BulletDynamics/Vehicle/btRaycastVehicle.cpp
	BulletDynamics/Vehicle/btWheelInfo.cpp
)


SET(BulletSoftBody_SRCS
	BulletSoftBody/btSoftBody.cpp
	BulletSoftBody/btSoftBodyConcaveCollisionAlgorithm.cpp
	BulletSoftBody/btSoftBodyHelpers.cpp
	BulletSoftBody/btSoftBodyRigidBodyCollisionConfiguration.cpp
	BulletSoftBody/btSoftRigidCollisionAlgorithm.cpp
	BulletSoftBody/btSoftRigidDynamicsWorld.cpp
	BulletSoftBody/btSoftSoftCollisionAlgorithm.cpp
)


SET(LinearMath_SRCS
	LinearMath/btAlignedAllocator.cpp
	LinearMath/btConvexHull.cpp
	LinearMath/btGeometryUtil.cpp
	LinearMath/btQuickprof.cpp
	LinearMath/btSerializer.cpp
)


SET(SRCS ${BulletCollision_SRCS}
         ${BulletDynamics_SRCS}
         ${BulletSoftBody_SRCS}
         ${LinearMath_SRCS}
)


# Declaration of the library
add_library(bullet STATIC ${SRCS})
