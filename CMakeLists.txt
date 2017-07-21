cmake_minimum_required(VERSION 3.0)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

set(idl_files 
	Header.idl
	Odometry.idl
	Point.idl
	Pose.idl
	PoseStamped.idl
	PoseWithCovariance.idl
	Quaternion.idl
	Time.idl
	Twist.idl
	TwistWithCovariance.idl
	Vector3.idl
)

set(idl_generated_files)

foreach(f ${idl_files})
	string(REPLACE ".idl" "" f_base ${f})
	list(APPEND idl_generated_files 
		${f_base}.h
		${f_base}.cxx
		${f_base}PubSubTypes.cxx
		${f_base}PubSubTypes.h)
endforeach()

add_custom_command(OUTPUT ${idl_generated_files}
	COMMAND fastrtpsgen -d ${CMAKE_CURRENT_BINARY_DIR} ${idl_files}
	DEPENDS ${DEPENDS} ${idl_files}
	COMMENT "Generating RTPS messages"
	VERBATIM
	WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)

add_library(rtps-topics ${idl_generated_files})
