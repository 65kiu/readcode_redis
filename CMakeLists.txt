cmake_minimum_required(VERSION 3.2 FATAL_ERROR)

project(redis VERSION 4.0.2)

message(STATUS "Host is: ${CMAKE_HOST_SYSTEM}.  Build target is: ${CMAKE_SYSTEM}")
get_filename_component(REDIS_ROOT "${CMAKE_CURRENT_SOURCE_DIR}" ABSOLUTE)
message(STATUS "Project root directory is: ${REDIS_ROOT}")

include_directories(deps/jemalloc/include/jemalloc deps/lua/src deps/hiredis)
include_directories(src)
add_executable(redis-server
 src/adlist.c
 src/quicklist.c
 src/ae.c
 src/anet.c
 src/dict.c
 src/server.c
 src/sds.c
 src/zmalloc.c
 src/lzf_c.c
 src/lzf_d.c
 src/pqsort.c
 src/zipmap.c
 src/sha1.c
 src/ziplist.c
 src/release.c
 src/networking.c
 src/util.c
 src/object.c
 src/db.c
 src/replication.c
 src/rdb.c
 src/t_string.c
 src/t_list.c
 src/t_set.c
 src/t_zset.c
 src/t_hash.c
 src/config.c
 src/aof.c
 src/pubsub.c
 src/multi.c
 src/debug.c
 src/sort.c
 src/intset.c
 src/syncio.c
 src/cluster.c
 src/crc16.c
 src/endianconv.c
 src/slowlog.c
 src/scripting.c
 src/bio.c
 src/rio.c
 src/rand.c
 src/memtest.c
 src/crc64.c
 src/bitops.c
 src/sentinel.c
 src/notify.c
 src/setproctitle.c
 src/blocked.c
 src/hyperloglog.c
 src/latency.c
 src/sparkline.c
 src/redis-check-rdb.c
 src/redis-check-aof.c
 src/geo.c
 src/lazyfree.c
 src/module.c
 src/evict.c
 src/expire.c
 src/geohash.c
 src/geohash_helper.c
 src/childinfo.c
 src/defrag.c
 src/siphash.c
 src/rax.c
 src/t_stream.c
 src/listpack.c
 src/localtime.c
 src/lolwut.c
 src/lolwut5.c
)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99 -pedantic -DREDIS_STATIC= -Wall -W -Wno-missing-field-initializers")

set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -g -ggdb -rdynamic")
set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)
target_link_libraries(redis-server PRIVATE Threads::Threads)
target_link_libraries(redis-server PRIVATE dl)

target_link_libraries(redis-server
        PRIVATE m
        PRIVATE ${REDIS_ROOT}/deps/lua/src/liblua.a
        PRIVATE ${REDIS_ROOT}/deps/linenoise/linenoise.o
        PRIVATE ${REDIS_ROOT}/deps/hiredis/libhiredis.a
        )

