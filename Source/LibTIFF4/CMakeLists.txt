cmake_minimum_required( VERSION 3.23 ) 

set( TIFFLIB_VERSION_MAJOR 4 )
set( TIFFLIB_VERSION_MINOR 6 )
set( TIFFLIB_VERSION_PATCH 0 )
set( TIFFLIB_VERSION ${TIFFLIB_VERSION_MAJOR}.${TIFFLIB_VERSION_MINOR}.${TIFFLIB_VERSION_PATCH} )

project( LibTIFF4 VERSION ${TIFFLIB_VERSION} LANGUAGES C )

include(CMakePackageConfigHelpers)

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/tif_config.h.cmake.in
               ${CMAKE_CURRENT_BINARY_DIR}/tif_config.h
               @ONLY)
               
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/tiffconf.h.cmake.in
               ${CMAKE_CURRENT_BINARY_DIR}/tiffconf.h
               @ONLY)

set( LIBTIFF_MAJOR_VERSION ${TIFFLIB_VERSION_MAJOR} )
set( LIBTIFF_MINOR_VERSION ${TIFFLIB_VERSION_MINOR} )
set( LIBTIFF_MICRO_VERSION ${TIFFLIB_VERSION_PATCH} )

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/tiffvers.h.in
               ${CMAKE_CURRENT_BINARY_DIR}/tiffvers.h
               @ONLY)


set( HEADER_FILES
	t4.h
	${CMAKE_CURRENT_BINARY_DIR}/tif_config.h
	tif_dir.h
	tif_fax3.h
	tif_hash_set.h
	tif_predict.h
	tiff.h
	${CMAKE_CURRENT_BINARY_DIR}/tiffconf.h
	tiffconf.vc.h
	tiffconf.wince.h
	tiffio.h
	tiffiop.h
	${CMAKE_CURRENT_BINARY_DIR}/tiffvers.h
	uvcode.h
)

set( SOURCE_FILES
	tif_aux.c
	tif_close.c
	tif_codec.c
	tif_color.c
	tif_compress.c
	tif_dir.c
	tif_dirinfo.c
	tif_dirread.c
	tif_dirwrite.c
	tif_dumpmode.c
	tif_error.c
	tif_extension.c
	tif_fax3.c
	tif_fax3sm.c
	tif_flush.c
	tif_getimage.c
	tif_hash_set.c
	tif_jpeg.c
	tif_lerc.c
	tif_luv.c
	tif_lzma.c
	tif_lzw.c
	tif_next.c
	tif_ojpeg.c
	tif_open.c
	tif_packbits.c
	tif_pixarlog.c
	tif_predict.c
	tif_print.c
	tif_read.c
	tif_strip.c
	tif_swab.c
	tif_thunder.c
	tif_tile.c
	tif_version.c
	tif_warning.c
	tif_webp.c
	tif_write.c
	tif_zip.c
)

add_library( LibTIFF4 STATIC ${HEADER_FILES} ${SOURCE_FILES} )

target_include_directories( LibTIFF4
	PUBLIC ./
)

target_link_libraries( LibTIFF4
	PRIVATE ZLibFreeImage LibJPEG
)