cmake_minimum_required( VERSION 3.23 )

# Version information
set( FREEIMAGE_VERSION_MAJOR 3 )
set( FREEIMAGE_VERSION_MINOR 19 )
set( FREEIMAGE_VERSION_PATCH 0 )
set( FREEIMAGE_VERSION_TWEAK 0 )
set( FREEIMAGE_VERSION ${FREEIMAGE_VERSION_MAJOR}.${FREEIMAGE_VERSION_MINOR}.${FREEIMAGE_VERSION_PATCH}.${FREEIMAGE_VERSION_TWEAK} )

project( FreeImage VERSION ${FREEIMAGE_VERSION} LANGUAGES C CXX )

add_subdirectory( ZLibFreeImage ) # ZLibFreeImage is a dependency of other libraries, so make sure it's loaded first.
add_subdirectory( LibJPEG )
add_subdirectory( LibJXR )
add_subdirectory( LibOpenJPEG )
add_subdirectory( LibPNG )
add_subdirectory( LibRawLite )
add_subdirectory( LibTIFF4 )
add_subdirectory( LibWebP )
add_subdirectory( OpenEXR )

set( HEADER_FILES
	CacheFile.h
	FreeImage.h
	FreeImageIO.h
	MapIntrospector.h
	Plugin.h
	Quantizers.h
	ToneMapping.h
	Utilities.h
	Metadata/FIRational.h
	Metadata/FreeImageTag.h
	FreeImage/PSDParser.h
	FreeImage/J2KHelper.h
)

source_group( "Header Files" ${HEADER_FILES} )

set( SOURCE_FILES
	FreeImage/BitmapAccess.cpp
	FreeImage/ColorLookup.cpp
	FreeImage/FreeImageC.c
	FreeImage/FreeImage.cpp
	FreeImage/FreeImageIO.cpp
	FreeImage/GetType.cpp
	FreeImage/MemoryIO.cpp
	FreeImage/PixelAccess.cpp
)

source_group( "Source Files" ${SOURCE_FILES} )

set( CONVERSION_SOURCE_FILES
	FreeImage/Conversion.cpp
	FreeImage/Conversion16_555.cpp
	FreeImage/Conversion16_565.cpp
	FreeImage/Conversion24.cpp
	FreeImage/Conversion32.cpp
	FreeImage/Conversion4.cpp
	FreeImage/Conversion8.cpp
	FreeImage/ConversionFloat.cpp
	FreeImage/ConversionRGB16.cpp
	FreeImage/ConversionRGBA16.cpp
	FreeImage/ConversionRGBAF.cpp
	FreeImage/ConversionRGBF.cpp
	FreeImage/ConversionType.cpp
	FreeImage/ConversionUINT16.cpp
	FreeImage/Halftoning.cpp
	FreeImage/tmoColorConvert.cpp
	FreeImage/tmoDrago03.cpp
	FreeImage/tmoFattal02.cpp
	FreeImage/tmoReinhard05.cpp
	FreeImage/ToneMapping.cpp
)

source_group("Source Files/Conversion" ${CONVERSION_SOURCE_FILES} )

set( METADATA_SOURCE_FILES
	Metadata/Exif.cpp
	Metadata/FIRational.cpp
	Metadata/FreeImageTag.cpp
	Metadata/IPTC.cpp
	Metadata/TagConversion.cpp
	Metadata/TagLib.cpp
	Metadata/XTIFF.cpp	
)

source_group( "Source Files/Metadata" ${METADATA_SOURCE_FILES} )

set( MULTIPAGING_SOURCE_FILES
	FreeImage/CacheFile.cpp
	FreeImage/MultiPage.cpp
	FreeImage/ZLibInterface.cpp
)

source_group( "Source Files/MultiPaging" ${MULTIPAGING_SOURCE_FILES} )

set( PLUGINS_SOURCE_FILES 
	FreeImage/J2KHelper.cpp
	FreeImage/MNGHelper.cpp
	FreeImage/Plugin.cpp
	FreeImage/PluginBMP.cpp
	FreeImage/PluginCUT.cpp
	FreeImage/PluginDDS.cpp
	FreeImage/PluginEXR.cpp
	FreeImage/PluginG3.cpp
	FreeImage/PluginGIF.cpp
	FreeImage/PluginHDR.cpp
	FreeImage/PluginICO.cpp
	FreeImage/PluginIFF.cpp
	FreeImage/PluginJ2K.cpp
	FreeImage/PluginJNG.cpp
	FreeImage/PluginJP2.cpp
	FreeImage/PluginJPEG.cpp
	FreeImage/PluginJXR.cpp
	FreeImage/PluginKOALA.cpp
	FreeImage/PluginMNG.cpp
	FreeImage/PluginPCD.cpp
	FreeImage/PluginPCX.cpp
	FreeImage/PluginPFM.cpp
	FreeImage/PluginPICT.cpp
	FreeImage/PluginPNG.cpp
	FreeImage/PluginPNM.cpp
	FreeImage/PluginPSD.cpp
	FreeImage/PluginRAS.cpp
	FreeImage/PluginRAW.cpp
	FreeImage/PluginSGI.cpp
	FreeImage/PluginTARGA.cpp
	FreeImage/PluginTIFF.cpp
	FreeImage/PluginWBMP.cpp
	FreeImage/PluginWebP.cpp
	FreeImage/PluginXBM.cpp
	FreeImage/PluginXPM.cpp
	FreeImage/PSDParser.cpp
	FreeImage/TIFFLogLuv.cpp
)

source_group( "Source Files/Plugins" ${PLUGINS_SOURCE_FILES} )

set( QUANTIZERS_SOURCE_FILES
	FreeImage/LFPQuantizer.cpp
	FreeImage/NNQuantizer.cpp
	FreeImage/WuQuantizer.cpp
)

source_group( "Source Files/Quantizers" ${QUANTIZERS_SOURCE_FILES} )

set( TOOLKIT_FILES
	FreeImageToolkit/Background.cpp
	FreeImageToolkit/BSplineRotate.cpp
	FreeImageToolkit/Channels.cpp
	FreeImageToolkit/ClassicRotate.cpp
	FreeImageToolkit/Colors.cpp
	FreeImageToolkit/CopyPaste.cpp
	FreeImageToolkit/Display.cpp
	FreeImageToolkit/Filters.h
	FreeImageToolkit/Flip.cpp
	FreeImageToolkit/JPEGTransform.cpp
	FreeImageToolkit/MultigridPoissonSolver.cpp
	FreeImageToolkit/Rescale.cpp
	FreeImageToolkit/Resize.cpp
	FreeImageToolkit/Resize.h
)

source_group( "Toolkit Files" ${TOOLKIT_FILES} )

set( ALL_FILES 
	${HEADER_FILES}
	${SOURCE_FILES}
	${CONVERSION_SOURCE_FILES}
	${METADATA_SOURCE_FILES}
	${MULTIPAGING_SOURCE_FILES}
	${PLUGINS_SOURCE_FILES}
	${QUANTIZERS_SOURCE_FILES}
	${TOOLKIT_FILES}
)

# Create shared library
add_library( FreeImage SHARED ${ALL_FILES} )

target_link_libraries( FreeImage
	PUBLIC LibJPEG LibJXR LibOpenJPEG LibPNG LibRaw LibTIFF4 LibWebP OpenEXR ZLibFreeImage
)

target_compile_definitions( FreeImage
	PRIVATE FREEIMAGE_EXPORTS
	INTERFACE FREEIMAGE_IMPORTS
)

target_include_directories( FreeImage
	PUBLIC ./
)

if(UNIX AND NOT APPLE)  # Only on Linux
    set_property(TARGET FreeImage PROPERTY POSITION_INDEPENDENT_CODE ON)
endif()

# Create static library
add_library( FreeImageLib STATIC ${ALL_FILES} )

target_link_libraries( FreeImageLib
	PUBLIC LibJPEG LibJXR LibOpenJPEG LibPNG LibRaw LibTIFF4 LibWebP OpenEXR ZLibFreeImage
)

target_compile_definitions( FreeImageLib
	PUBLIC FREEIMAGE_LIB
)

target_include_directories( FreeImageLib
	PUBLIC ./
)

set(TARGET_LIST FreeImage FreeImageLib LibJPEG LibJXR LibOpenJPEG LibPNG LibRaw LibTIFF4 LibWebP OpenEXR ZLibFreeImage)

set_target_properties(FreeImage PROPERTIES VERSION ${FREEIMAGE_VERSION_MAJOR}.${FREEIMAGE_VERSION_MINOR}.${FREEIMAGE_VERSION_PATCH})
set_target_properties(FreeImageLib PROPERTIES VERSION ${FREEIMAGE_VERSION_MAJOR}.${FREEIMAGE_VERSION_MINOR}.${FREEIMAGE_VERSION_PATCH})

# Enforce and cxx standards
set(FreeImage_CXX_STANDARD 14)
set(FreeImage_C_STANDARD 11)

# MSVC 2019 does not seem to support c11 nicely. use it for all other compilers though
# target_compile_features c_std_11 let's a target use newer versions if available
if (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC" AND MSVC_VERSION LESS 1930)
    set(FreeImage_C_STANDARD 99)
endif()

foreach(target IN LISTS TARGET_LIST)
    set_target_properties(${target} PROPERTIES CXX_STANDARD ${FreeImage_CXX_STANDARD})
    set_target_properties(${target} PROPERTIES C_STANDARD ${FreeImage_C_STANDARD})
endforeach()

# msvc specific compile options
if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    foreach(target IN LISTS TARGET_LIST)
        target_compile_options(${target} PRIVATE /EHsc /Zc:__cplusplus)
    endforeach()
endif()

# windows specific compile definitions
if(WIN32)
    foreach(target IN LISTS TARGET_LIST)
        target_compile_definitions(${target} PRIVATE WIN32)
    endforeach()
endif()

# position independent code on linux
if(UNIX AND NOT APPLE)
    message(STATUS "[LINUX] Setting: POSITION_INDEPENDENT_CODE ON")
    foreach(target IN LISTS TARGET_LIST)
        set_property(TARGET ${target} PROPERTY POSITION_INDEPENDENT_CODE ON)
    endforeach()
endif()

# control exception handling and symbol visibility 
if(UNIX)
    message(STATUS "[UNIX] Setting: __ANSI__")
    message(STATUS "[UNIX] Using: -fexceptions -fvisibility=hidden")
    foreach(target IN LISTS TARGET_LIST)
        target_compile_options(${target} PRIVATE -fexceptions -fvisibility=hidden)
        target_compile_definitions(${target} PRIVATE __ANSI__)
    endforeach()
endif()

# apple specific compile definitions
if(APPLE)
    message(STATUS "[APPLE] Setting: NO_LCMS")
    foreach(target IN LISTS TARGET_LIST)
        target_compile_definitions(${target} PRIVATE NO_LCMS)
    endforeach()
endif()

# Temporary workaround for ARM, see https://github.com/Homebrew/homebrew-core/blob/b5d2dd8e69b2729015a2855060c1f2dfa303dbdc/Formula/f/freeimage.rb
if(CMAKE_SYSTEM_PROCESSOR MATCHES "^(arm|Arm|ARM|aarch64|AAarch64|AARCH64)")
    message(STATUS "[ARM] Setting: PNG_ARM_NEON_OPT=0")
    foreach(target IN LISTS TARGET_LIST)
        target_compile_definitions(${target} PRIVATE PNG_ARM_NEON_OPT=0)
    endforeach()
endif()

# Temporary workaround for potential issues with clang, see https://github.com/Homebrew/homebrew-core/blob/b5d2dd8e69b2729015a2855060c1f2dfa303dbdc/Formula/f/freeimage.rb
if(CMAKE_CXX_COMPILER_ID MATCHES "^(Clang|AppleClang)" AND CMAKE_CXX_COMPILER_VERSION VERSION_GREATER_EQUAL 15)
    message(STATUS "[CLANG>=15] Using: -Wno-implicit-function-declaration")
    foreach(target IN LISTS TARGET_LIST)
        target_compile_options(${target} PRIVATE -Wno-implicit-function-declaration)
    endforeach()
endif()
