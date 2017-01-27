project(darling-liblzma)

cmake_minimum_required(VERSION 2.4.0)
enable_language(C ASM)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -msse -msse2 -msse3 -w -nostdinc")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D__DARWIN_UNIX03 -fPIC -w")

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/liblzma/api)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/liblzma/check)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/liblzma/common)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/liblzma/lzma)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/liblzma/lz)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/liblzma/simple)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/liblzma/delta)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/liblzma/rangecoder)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/common)
include_directories(${DARLING_TOP_DIRECTORY}/platform-include)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/fbsdcompat)
add_definitions(-D_LARGEFILE64_SOURCE=1 -DHAVE_CONFIG_H=1 -DMYTHREAD_POSIX=1)
add_definitions(-DTUKLIB_PHYSMEM_SYSCTL=1 -DTUKLIB_CPUCORES_SYSCTL=1)

set(lzma_sources
	src/liblzma/common/alone_decoder.c
	src/liblzma/common/alone_encoder.c
	src/liblzma/common/auto_decoder.c
	src/liblzma/common/block_buffer_decoder.c
	src/liblzma/common/block_buffer_encoder.c
	src/liblzma/common/block_decoder.c
	src/liblzma/common/block_encoder.c
	src/liblzma/common/block_header_decoder.c
	src/liblzma/common/block_header_encoder.c
	src/liblzma/common/block_util.c
	src/liblzma/common/common.c
	src/liblzma/common/easy_buffer_encoder.c
	src/liblzma/common/easy_decoder_memusage.c
	src/liblzma/common/easy_encoder.c
	src/liblzma/common/easy_encoder_memusage.c
	src/liblzma/common/easy_preset.c
	src/liblzma/common/filter_buffer_decoder.c
	src/liblzma/common/filter_buffer_encoder.c
	src/liblzma/common/filter_common.c
	src/liblzma/common/filter_decoder.c
	src/liblzma/common/filter_encoder.c
	src/liblzma/common/filter_flags_decoder.c
	src/liblzma/common/filter_flags_encoder.c
	src/liblzma/common/hardware_cputhreads.c
	src/liblzma/common/hardware_physmem.c
	src/liblzma/common/index.c
	src/liblzma/common/index_decoder.c
	src/liblzma/common/index_encoder.c
	src/liblzma/common/index_hash.c
	src/liblzma/common/outqueue.c
	src/liblzma/common/stream_buffer_decoder.c
	src/liblzma/common/stream_buffer_encoder.c
	src/liblzma/common/stream_decoder.c
	src/liblzma/common/stream_encoder.c
	src/liblzma/common/stream_encoder_mt.c
	src/liblzma/common/stream_flags_common.c
	src/liblzma/common/stream_flags_decoder.c
	src/liblzma/common/stream_flags_encoder.c
	src/liblzma/common/vli_decoder.c
	src/liblzma/common/vli_encoder.c
	src/liblzma/common/vli_size.c

	src/liblzma/simple/arm.c
	src/liblzma/simple/armthumb.c
	src/liblzma/simple/ia64.c
	src/liblzma/simple/powerpc.c
	src/liblzma/simple/simple_coder.c
	src/liblzma/simple/simple_decoder.c
	src/liblzma/simple/simple_encoder.c
	src/liblzma/simple/sparc.c
	src/liblzma/simple/x86.c

	src/liblzma/lz/lz_decoder.c
	src/liblzma/lz/lz_encoder.c
	src/liblzma/lz/lz_encoder_mf.c

	src/liblzma/lzma/fastpos_table.c
	#src/liblzma/lzma/fastpos_tablegen.c
	src/liblzma/lzma/lzma2_decoder.c
	src/liblzma/lzma/lzma2_encoder.c
	src/liblzma/lzma/lzma_decoder.c
	src/liblzma/lzma/lzma_encoder.c
	src/liblzma/lzma/lzma_encoder_optimum_fast.c
	src/liblzma/lzma/lzma_encoder_optimum_normal.c
	src/liblzma/lzma/lzma_encoder_presets.c

	src/liblzma/rangecoder/price_table.c
	#src/liblzma/rangecoder/price_tablegen.c

	src/liblzma/delta/delta_common.c
	src/liblzma/delta/delta_decoder.c
	src/liblzma/delta/delta_encoder.c

	src/liblzma/check/check.c
	src/liblzma/check/crc32_fast.c
	src/liblzma/check/crc32_table.c
	#src/liblzma/check/crc32_tablegen.c
	src/liblzma/check/crc64_fast.c
	src/liblzma/check/crc64_table.c
	#src/liblzma/check/crc64_tablegen.c
	src/liblzma/check/sha256.c

	src/common/tuklib_cpucores.c
	src/common/tuklib_exit.c
	src/common/tuklib_mbstr_fw.c
	src/common/tuklib_mbstr_width.c
	src/common/tuklib_open_stdxxx.c
	src/common/tuklib_physmem.c
	src/common/tuklib_progname.c
)

set(DYLIB_INSTALL_NAME "/usr/lib/liblzma.5.dylib")
add_darling_library(lzma SHARED ${lzma_sources})
make_fat(lzma)
target_link_libraries(lzma system)
set_target_properties(lzma PROPERTIES OUTPUT_NAME "lzma.5")

InstallSymlink("liblzma.5.dylib" "${CMAKE_INSTALL_PREFIX}/libexec/darling/usr/lib/liblzma.dylib")
install(TARGETS lzma DESTINATION libexec/darling/usr/lib)

install(DIRECTORY usr DESTINATION libexec/darling)

