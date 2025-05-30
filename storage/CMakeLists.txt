if (NOT OPENSSL_FOUND)
  find_package(OpenSSL REQUIRED)
endif()


set(STORAGE_SOURCE
  LoadSpeed.cpp
  MerkleTree.cpp
  MicrochunkTree.cpp
  NodeActor.cpp
  PeerActor.cpp
  PeerState.cpp
  SpeedLimiter.cpp
  Torrent.cpp
  TorrentCreator.cpp
  TorrentHeader.cpp
  TorrentInfo.cpp
  TorrentMeta.cpp

  db.h
  Bitset.h
  LoadSpeed.h
  MerkleTree.h
  NodeActor.h
  PartsHelper.h
  PeerActor.h
  PeerState.h
  SpeedLimiter.h
  Torrent.h
  TorrentCreator.h
  TorrentHeader.h
  TorrentInfo.h
  TorrentMeta.h
  PeerManager.h
  MicrochunkTree.h)
set(STORAGE_CLI_SOURCE
  storage-cli.cpp
)

add_library(storage STATIC ${STORAGE_SOURCE})
target_link_libraries(storage tdutils tdactor tddb ton_crypto tl_api ${JEMALLOC_LIBRARIES})
target_include_directories(storage PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
)

add_executable(storage-cli ${STORAGE_CLI_SOURCE})
target_link_libraries(storage-cli storage overlay tdutils tdactor adnl tl_api dht rldp rldp2 fift-lib memprof terminal git ${JEMALLOC_LIBRARIES})

set(STORAGE_TEST_SOURCE
  ${CMAKE_CURRENT_SOURCE_DIR}/test/storage.cpp
  PARENT_SCOPE
)

add_subdirectory(storage-daemon)

# Do not install it yet
install(TARGETS storage-cli storage-daemon storage-daemon-cli RUNTIME DESTINATION bin)
