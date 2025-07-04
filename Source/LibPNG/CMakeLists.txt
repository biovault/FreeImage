cmake_minimum_required(VERSION 3.23.0 )

set(PNGLIB_MAJOR 1)
set(PNGLIB_MINOR 6)
set(PNGLIB_REVISION 48)
set(PNGLIB_SUBREVISION 0)
set(PNGLIB_NAME "libpng${PNGLIB_MAJOR}${PNGLIB_MINOR}")
set(PNGLIB_VERSION "${PNGLIB_MAJOR}.${PNGLIB_MINOR}.${PNGLIB_REVISION}")
set(PNGLIB_SHARED_SOVERSION "${PNGLIB_MAJOR}${PNGLIB_MINOR}")
set(PNGLIB_SHARED_VERSION "${PNGLIB_SHARED_SOVERSION}.${PNGLIB_REVISION}.${PNGLIB_SUBREVISION}")

project(LibPNG VERSION ${PNGLIB_VERSION} LANGUAGES C ASM)

set( HEADER_FILES
	png.h
	pngconf.h
	pngdebug.h
	pnginfo.h
	pnglibconf.h
	pngpriv.h
	pngstruct.h
)

set( SOURCE_FILES
	png.c
	pngerror.c
	pngget.c
	pngmem.c
	pngpread.c
	pngread.c
	pngrio.c
	pngrtran.c
	pngrutil.c
	pngset.c
	pngtest.c
	pngtrans.c
	pngwio.c
	pngwrite.c
	pngwtran.c
	pngwutil.c
)

add_library( LibPNG STATIC ${HEADER_FILES} ${SOURCE_FILES} )

target_link_libraries( LibPNG
	PUBLIC ZLibFreeImage
)