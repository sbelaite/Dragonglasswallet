cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB dragonglass)

include_directories(${CMAKE_BINARY_DIR}
    src
    dragonglass/external
    dragonglass/include
    dragonglass/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("dragonglass/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  dragonglass/external/miniupnpc/connecthostport.c
  dragonglass/external/miniupnpc/igd_desc_parse.c
  dragonglass/external/miniupnpc/minisoap.c
  dragonglass/external/miniupnpc/miniupnpc.c
  dragonglass/external/miniupnpc/miniwget.c
  dragonglass/external/miniupnpc/minixml.c
  dragonglass/external/miniupnpc/portlistingparse.c
  dragonglass/external/miniupnpc/receivedata.c
  dragonglass/external/miniupnpc/upnpcommands.c
  dragonglass/external/miniupnpc/upnpreplyparse.c
  dragonglass/src/Common/Base58.cpp
  dragonglass/src/Common/CommandLine.cpp
  dragonglass/src/Common/Util.cpp
  dragonglass/src/Common/StringTools.cpp
  dragonglass/src/Common/JsonValue.cpp
  dragonglass/src/Common/ConsoleTools.cpp
  dragonglass/src/Common/MemoryInputStream.cpp
  dragonglass/src/Common/PathTools.cpp
  dragonglass/src/Common/StdInputStream.cpp
  dragonglass/src/Common/StdOutputStream.cpp
  dragonglass/src/Common/StreamTools.cpp
  dragonglass/src/Common/StringOutputStream.cpp
  dragonglass/src/Common/StringView.cpp
  dragonglass/src/Common/VectorOutputStream.cpp
  dragonglass/src/crypto/blake256.c
  dragonglass/src/crypto/chacha8.c
  dragonglass/src/crypto/crypto-ops-data.c
  dragonglass/src/crypto/crypto-ops.c
  dragonglass/src/crypto/crypto.cpp
  dragonglass/src/crypto/groestl.c
  dragonglass/src/crypto/hash-extra-blake.c
  dragonglass/src/crypto/hash-extra-groestl.c
  dragonglass/src/crypto/hash-extra-jh.c
  dragonglass/src/crypto/hash-extra-skein.c
  dragonglass/src/crypto/hash.c
  dragonglass/src/crypto/jh.c
  dragonglass/src/crypto/keccak.c
  dragonglass/src/crypto/oaes_lib.c
  dragonglass/src/crypto/random.c
  dragonglass/src/crypto/skein.c
  dragonglass/src/crypto/slow-hash.c
  dragonglass/src/crypto/slow-hash.cpp
  dragonglass/src/crypto/tree-hash.c
  dragonglass/src/CryptoNoteCore/BlockchainIndices.cpp
  dragonglass/src/CryptoNoteCore/BlockchainMessages.cpp
  dragonglass/src/CryptoNoteCore/BlockIndex.cpp
  dragonglass/src/CryptoNoteCore/CoreConfig.cpp
  dragonglass/src/CryptoNoteCore/CryptoNoteBasic.cpp
  dragonglass/src/CryptoNoteCore/CryptoNoteTools.cpp
  dragonglass/src/CryptoNoteCore/Currency.cpp
  dragonglass/src/CryptoNoteCore/MinerConfig.cpp
  dragonglass/src/CryptoNoteCore/Transaction.cpp
  dragonglass/src/CryptoNoteCore/Account.cpp
  dragonglass/src/CryptoNoteCore/Blockchain.cpp
  dragonglass/src/CryptoNoteCore/Checkpoints.cpp
  dragonglass/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  dragonglass/src/CryptoNoteCore/Core.cpp
  dragonglass/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  dragonglass/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  dragonglass/src/CryptoNoteCore/Difficulty.cpp
  dragonglass/src/CryptoNoteCore/IBlock.cpp
  dragonglass/src/CryptoNoteCore/Miner.cpp
  dragonglass/src/CryptoNoteCore/TransactionExtra.cpp
  dragonglass/src/CryptoNoteCore/TransactionPool.cpp
  dragonglass/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  dragonglass/src/CryptoNoteCore/TransactionUtils.cpp
  dragonglass/src/InProcessNode/InProcessNode.cpp
  dragonglass/src/InProcessNode/InProcessNodeErrors.cpp
  dragonglass/src/NodeRpcProxy/NodeErrors.cpp
  dragonglass/src/NodeRpcProxy/NodeRpcProxy.cpp
  dragonglass/src/P2p/NetNodeConfig.cpp
  dragonglass/src/Serialization/BinaryInputStreamSerializer.cpp
  dragonglass/src/Serialization/BinaryOutputStreamSerializer.cpp
  dragonglass/src/Serialization/JsonInputValueSerializer.cpp
  dragonglass/src/Serialization/JsonOutputStreamSerializer.cpp
  dragonglass/src/Serialization/KVBinaryInputStreamSerializer.cpp
  dragonglass/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  dragonglass/src/Serialization/SerializationOverloads.cpp
  dragonglass/src/Transfers/BlockchainSynchronizer.cpp
  dragonglass/src/Transfers/SynchronizationState.cpp
  dragonglass/src/Transfers/TransfersConsumer.cpp
  dragonglass/src/Transfers/TransfersContainer.cpp
  dragonglass/src/Transfers/TransfersSubscription.cpp
  dragonglass/src/Transfers/TransfersSynchronizer.cpp
  dragonglass/src/Wallet/LegacyKeysImporter.cpp
  dragonglass/src/Wallet/WalletAsyncContextCounter.cpp
  dragonglass/src/Wallet/WalletErrors.cpp
  dragonglass/src/WalletLegacy/KeysStorage.cpp
  dragonglass/src/WalletLegacy/WalletLegacy.cpp
  dragonglass/src/WalletLegacy/WalletHelper.cpp
  dragonglass/src/WalletLegacy/WalletLegacySerializer.cpp
  dragonglass/src/WalletLegacy/WalletLegacySerialization.cpp
  dragonglass/src/WalletLegacy/WalletTransactionSender.cpp
  dragonglass/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  dragonglass/src/WalletLegacy/WalletUserTransactionsCache.cpp
  dragonglass/src/System/ContextGroup.cpp
  dragonglass/src/System/Event.cpp
  dragonglass/src/System/EventLock.cpp
  dragonglass/src/System/InterruptedException.cpp
  dragonglass/src/System/Ipv4Address.cpp
  dragonglass/src/System/TcpStream.cpp
  dragonglass/src/HTTP/HttpRequest.cpp
  dragonglass/src/HTTP/HttpParser.cpp
  dragonglass/src/HTTP/HttpParserErrorCodes.cpp
  dragonglass/src/HTTP/HttpResponse.cpp
  dragonglass/src/Rpc/HttpClient.cpp
  dragonglass/src/Rpc/JsonRpc.cpp
  dragonglass/src/P2p/NetNode.cpp
  dragonglass/src/P2p/LevinProtocol.cpp
  dragonglass/src/P2p/NetNodeConfig.cpp
  dragonglass/src/P2p/PeerListManager.cpp
  dragonglass/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  dragonglass/src/Logging/ILogger.cpp
  dragonglass/src/Logging/LoggerGroup.cpp
  dragonglass/src/Logging/CommonLogger.cpp
  dragonglass/src/Logging/LoggerManager.cpp
  dragonglass/src/Logging/FileLogger.cpp
  dragonglass/src/Logging/StreamLogger.cpp
  dragonglass/src/Logging/ConsoleLogger.cpp
  dragonglass/src/Logging/LoggerMessage.cpp
  dragonglass/src/Logging/LoggerRef.cpp
  dragonglass/src/BlockchainExplorer/BlockchainExplorer.cpp
  dragonglass/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  dragonglass/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(dragonglass/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} dragonglass/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(dragonglass/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES dragonglass/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
