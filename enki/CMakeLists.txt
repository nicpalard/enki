include_directories (${PROJECT_SOURCE_DIR})

if (CMAKE_HOST_UNIX)
	add_definitions("-fPIC")
endif (CMAKE_HOST_UNIX)

add_library(enki
	Geometry.cpp
	Types.cpp
	PhysicalEngine.cpp
	BluetoothBase.cpp
	Factory.cpp
	Serializer.cpp
	Network.cpp
	interactions/IRSensor.cpp
	interactions/GroundSensor.cpp
	interactions/CircularCam.cpp
	interactions/Bluetooth.cpp
	interactions/ActiveSoundSource.cpp
	interactions/Microphone.cpp
	robots/DifferentialWheeled.cpp
	robots/khepera/Khepera.cpp
	robots/e-puck/EPuck.cpp
	robots/s-bot/Sbot.cpp
	robots/s-bot/SbotObject.cpp
	robots/marxbot/Marxbot.cpp
	robots/thymio2/Thymio2.cpp
)

add_library(worldgenerator
	worldgenerator/Randomizer.cpp
	worldgenerator/Randomizer.h
	worldgenerator/WorldGenerator.cpp
	worldgenerator/WorldGenerator.h
)
set_target_properties(enki PROPERTIES VERSION ${LIB_VERSION_STRING}
                                        SOVERSION ${LIB_VERSION_MAJOR})

install(DIRECTORY .
	DESTINATION include/enki/
	FILES_MATCHING PATTERN "*.h"
)
install(TARGETS enki LIBRARY DESTINATION ${LIB_INSTALL_DIR}
                     ARCHIVE DESTINATION ${LIB_INSTALL_DIR})
