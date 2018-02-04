cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB firstcryptobank)

include_directories(${CMAKE_BINARY_DIR}
    src
    firstcryptobank/external
    firstcryptobank/include
    firstcryptobank/src)

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
configure_file("firstcryptobank/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  firstcryptobank/external/miniupnpc/connecthostport.c
  firstcryptobank/external/miniupnpc/igd_desc_parse.c
  firstcryptobank/external/miniupnpc/minisoap.c
  firstcryptobank/external/miniupnpc/miniupnpc.c
  firstcryptobank/external/miniupnpc/miniwget.c
  firstcryptobank/external/miniupnpc/minixml.c
  firstcryptobank/external/miniupnpc/portlistingparse.c
  firstcryptobank/external/miniupnpc/receivedata.c
  firstcryptobank/external/miniupnpc/upnpcommands.c
  firstcryptobank/external/miniupnpc/upnpreplyparse.c
  firstcryptobank/src/Common/Base58.cpp
  firstcryptobank/src/Common/CommandLine.cpp
  firstcryptobank/src/Common/Util.cpp
  firstcryptobank/src/Common/StringTools.cpp
  firstcryptobank/src/Common/JsonValue.cpp
  firstcryptobank/src/Common/ConsoleTools.cpp
  firstcryptobank/src/Common/MemoryInputStream.cpp
  firstcryptobank/src/Common/PathTools.cpp
  firstcryptobank/src/Common/StdInputStream.cpp
  firstcryptobank/src/Common/StdOutputStream.cpp
  firstcryptobank/src/Common/StreamTools.cpp
  firstcryptobank/src/Common/StringOutputStream.cpp
  firstcryptobank/src/Common/StringView.cpp
  firstcryptobank/src/Common/VectorOutputStream.cpp
  firstcryptobank/src/crypto/blake256.c
  firstcryptobank/src/crypto/chacha8.c
  firstcryptobank/src/crypto/crypto-ops-data.c
  firstcryptobank/src/crypto/crypto-ops.c
  firstcryptobank/src/crypto/crypto.cpp
  firstcryptobank/src/crypto/groestl.c
  firstcryptobank/src/crypto/hash-extra-blake.c
  firstcryptobank/src/crypto/hash-extra-groestl.c
  firstcryptobank/src/crypto/hash-extra-jh.c
  firstcryptobank/src/crypto/hash-extra-skein.c
  firstcryptobank/src/crypto/hash.c
  firstcryptobank/src/crypto/jh.c
  firstcryptobank/src/crypto/keccak.c
  firstcryptobank/src/crypto/oaes_lib.c
  firstcryptobank/src/crypto/random.c
  firstcryptobank/src/crypto/skein.c
  firstcryptobank/src/crypto/slow-hash.c
  firstcryptobank/src/crypto/slow-hash.cpp
  firstcryptobank/src/crypto/tree-hash.c
  firstcryptobank/src/CryptoNoteCore/BlockchainIndices.cpp
  firstcryptobank/src/CryptoNoteCore/BlockchainMessages.cpp
  firstcryptobank/src/CryptoNoteCore/BlockIndex.cpp
  firstcryptobank/src/CryptoNoteCore/CoreConfig.cpp
  firstcryptobank/src/CryptoNoteCore/CryptoNoteBasic.cpp
  firstcryptobank/src/CryptoNoteCore/CryptoNoteTools.cpp
  firstcryptobank/src/CryptoNoteCore/Currency.cpp
  firstcryptobank/src/CryptoNoteCore/MinerConfig.cpp
  firstcryptobank/src/CryptoNoteCore/Transaction.cpp
  firstcryptobank/src/CryptoNoteCore/Account.cpp
  firstcryptobank/src/CryptoNoteCore/Blockchain.cpp
  firstcryptobank/src/CryptoNoteCore/Checkpoints.cpp
  firstcryptobank/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  firstcryptobank/src/CryptoNoteCore/Core.cpp
  firstcryptobank/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  firstcryptobank/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  firstcryptobank/src/CryptoNoteCore/Difficulty.cpp
  firstcryptobank/src/CryptoNoteCore/IBlock.cpp
  firstcryptobank/src/CryptoNoteCore/Miner.cpp
  firstcryptobank/src/CryptoNoteCore/TransactionExtra.cpp
  firstcryptobank/src/CryptoNoteCore/TransactionPool.cpp
  firstcryptobank/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  firstcryptobank/src/CryptoNoteCore/TransactionUtils.cpp
  firstcryptobank/src/InProcessNode/InProcessNode.cpp
  firstcryptobank/src/InProcessNode/InProcessNodeErrors.cpp
  firstcryptobank/src/NodeRpcProxy/NodeErrors.cpp
  firstcryptobank/src/NodeRpcProxy/NodeRpcProxy.cpp
  firstcryptobank/src/P2p/NetNodeConfig.cpp
  firstcryptobank/src/Serialization/BinaryInputStreamSerializer.cpp
  firstcryptobank/src/Serialization/BinaryOutputStreamSerializer.cpp
  firstcryptobank/src/Serialization/JsonInputValueSerializer.cpp
  firstcryptobank/src/Serialization/JsonOutputStreamSerializer.cpp
  firstcryptobank/src/Serialization/KVBinaryInputStreamSerializer.cpp
  firstcryptobank/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  firstcryptobank/src/Serialization/SerializationOverloads.cpp
  firstcryptobank/src/Transfers/BlockchainSynchronizer.cpp
  firstcryptobank/src/Transfers/SynchronizationState.cpp
  firstcryptobank/src/Transfers/TransfersConsumer.cpp
  firstcryptobank/src/Transfers/TransfersContainer.cpp
  firstcryptobank/src/Transfers/TransfersSubscription.cpp
  firstcryptobank/src/Transfers/TransfersSynchronizer.cpp
  firstcryptobank/src/Wallet/LegacyKeysImporter.cpp
  firstcryptobank/src/Wallet/WalletAsyncContextCounter.cpp
  firstcryptobank/src/Wallet/WalletErrors.cpp
  firstcryptobank/src/WalletLegacy/KeysStorage.cpp
  firstcryptobank/src/WalletLegacy/WalletLegacy.cpp
  firstcryptobank/src/WalletLegacy/WalletHelper.cpp
  firstcryptobank/src/WalletLegacy/WalletLegacySerializer.cpp
  firstcryptobank/src/WalletLegacy/WalletLegacySerialization.cpp
  firstcryptobank/src/WalletLegacy/WalletTransactionSender.cpp
  firstcryptobank/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  firstcryptobank/src/WalletLegacy/WalletUserTransactionsCache.cpp
  firstcryptobank/src/System/ContextGroup.cpp
  firstcryptobank/src/System/Event.cpp
  firstcryptobank/src/System/EventLock.cpp
  firstcryptobank/src/System/InterruptedException.cpp
  firstcryptobank/src/System/Ipv4Address.cpp
  firstcryptobank/src/System/TcpStream.cpp
  firstcryptobank/src/HTTP/HttpRequest.cpp
  firstcryptobank/src/HTTP/HttpParser.cpp
  firstcryptobank/src/HTTP/HttpParserErrorCodes.cpp
  firstcryptobank/src/HTTP/HttpResponse.cpp
  firstcryptobank/src/Rpc/HttpClient.cpp
  firstcryptobank/src/Rpc/JsonRpc.cpp
  firstcryptobank/src/P2p/NetNode.cpp
  firstcryptobank/src/P2p/LevinProtocol.cpp
  firstcryptobank/src/P2p/NetNodeConfig.cpp
  firstcryptobank/src/P2p/PeerListManager.cpp
  firstcryptobank/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  firstcryptobank/src/Logging/ILogger.cpp
  firstcryptobank/src/Logging/LoggerGroup.cpp
  firstcryptobank/src/Logging/CommonLogger.cpp
  firstcryptobank/src/Logging/LoggerManager.cpp
  firstcryptobank/src/Logging/FileLogger.cpp
  firstcryptobank/src/Logging/StreamLogger.cpp
  firstcryptobank/src/Logging/ConsoleLogger.cpp
  firstcryptobank/src/Logging/LoggerMessage.cpp
  firstcryptobank/src/Logging/LoggerRef.cpp
  firstcryptobank/src/BlockchainExplorer/BlockchainExplorer.cpp
  firstcryptobank/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  firstcryptobank/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
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
  include_directories(firstcryptobank/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} firstcryptobank/external/miniupnpc/minissdpc.c)
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

include_directories(firstcryptobank/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES firstcryptobank/src/Platform/${PLATFORM_DIR}/System/*)
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
