cmake_minimum_required( VERSION 3.23 ) 

project( LibOpenJPEG LANGUAGES C )

set( HEADER_FILES
	bio.h
	cidx_manager.h
	cio.h
	dwt.h
	event.h
	function_list.h
	image.h
	indexbox_manager.h
	invert.h
	j2k.h
	jp2.h
	mct.h
	mqc.h
	openjpeg.h
	opj_clock.h
	opj_codec.h
	opj_config.h
	opj_config_private.h
	opj_includes.h
	opj_intmath.h
	opj_inttypes.h
	opj_malloc.h
	opj_stdint.h
	pi.h
	raw.h
	t1.h
	t1_luts.h
	t2.h
	tcd.h
	tgt.h
)

set( SOURCE_FILES
	bio.c
#	cidx_manager.c
	cio.c
	dwt.c
	event.c
	function_list.c
	image.c
	invert.c
	j2k.c
	jp2.c
	mct.c
	mqc.c
	openjpeg.c
	opj_clock.c
#	phix_manager.c
	pi.c
#	ppix_manager.c
	raw.c
	t1.c
#	t1_generate_luts.c
	t2.c
	tcd.c
	tgt.c
#	thix_manager.c
#	tpix_manager.c
)

add_library( LibOpenJPEG STATIC ${HEADER_FILES} ${SOURCE_FILES} )

target_compile_definitions( LibOpenJPEG 
	PUBLIC OPJ_STATIC
)