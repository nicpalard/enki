# Testing for Enki

include_directories(${PROJECT_SOURCE_DIR})
link_directories(${PROJECT_BINARY_DIR})

# Contain Catch testing framework main function.  Using CATCH_CONFIG_MAIN macro
# only this library reduce compilation time.
add_library(catch STATIC
	catch.cpp
	catch.hpp
)

# BDD test example.
add_executable(enkiTestBDD
        enkiTestBDD.cpp
)
target_link_libraries(enkiTestBDD enki catch)

add_test(enki-test enkiTestBDD "An Enki world may contain a robot")

# Test the World class.
add_executable(enkiTestWorld enkiTestWorld.cpp)
target_link_libraries(enkiTestWorld enki catch)

add_test(enki_test_empty_world enkiTestWorld "An empty Enki world")
add_test(enki_test_thymio enkiTestWorld "Thymio2")
add_test(enki_test_EPuck enkiTestWorld "EPuck")
add_test(enki_test_Marxbot enkiTestWorld "Marxbot")
add_test(enki_test_Khepera enkiTestWorld "Khepera")
add_test(enki_test_Sbot enkiTestWorld "S-Bot")

add_executable(randomWorld randomWorld.cpp)
target_link_libraries(randomWorld worldgenerator catch enki)

add_test(random_world randomWorld)

add_executable(serialization serialization.cpp)
target_link_libraries(serialization worldgenerator catch enki)

add_test(serialize serialization "Unitary TEST_CASE on serialization")

add_executable(serializationRT serializationRT.cpp)
target_link_libraries(serializationRT catch enki worldgenerator)
add_test(serializationRT serializationRT "Robustness Test Serialization")

add_executable(serializationBenchmarks serializationBenchmarks.cpp)
target_link_libraries(serializationBenchmarks catch enki)

#add_test(BenchmarksSize serializationBenchmarks "BenchMarks Serialization Size")
#add_test(BenchmarksSerializationTime serializationBenchmarks "BenchMarks Time Serialization")

add_executable(network network.cpp)
target_link_libraries(network enki ${dashel_LIBRARIES} ${CMAKE_THREAD_LIBS_INIT} catch)

set(QT_USE_QTOPENGL TRUE)
find_package(Qt4)
find_package(OpenGL)
include(${QT_USE_FILE})
add_definitions(${QT_DEFINITIONS})

add_executable(viewworld testGenerator.cpp)
target_link_libraries(viewworld worldgenerator enki enkiviewer ${OPENGL_LIBRARIES} ${QT_LIBRARIES} ${EXTRA_LIBS})


if (WIN32)
	add_test(network_basic network "Scenario: Connection Deconnection" WILL_FAIL)
else()
	add_test(network_basic network "Scenario: Connection Deconnection")
endif()
