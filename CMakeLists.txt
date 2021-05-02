cmake_minimum_required(VERSION 3.10)
project(CVD)
enable_testing()

include(TestBigEndian)
include(CheckSymbolExists)

set(CMAKE_DEBUG_POSTFIX _debug)

set(LIBCVD_PATH "libcvd/")

# Dependencies that can be automatically found by CMake.
include(${LIBCVD_PATH}cmake/CVDFindAllDeps.cmake)

# Detect endianness

TEST_BIG_ENDIAN(CVD_INTERNAL_ARCH_BIG_ENDIAN)
if(CVD_INTERNAL_ARCH_BIG_ENDIAN)
	set(CVD_INTERNAL_ARCH_LITTLE_ENDIAN OFF)
else()
	set(CVD_INTERNAL_ARCH_LITTLE_ENDIAN ON)
endif()

# Basic source files and headers for all platforms and options.



set(SRCS
	${LIBCVD_PATH}cvd_src/bayer.cxx
	${LIBCVD_PATH}cvd_src/colourspace_convert.cxx
	${LIBCVD_PATH}cvd_src/connected_components.cc
	${LIBCVD_PATH}cvd_src/convolution.cc
	${LIBCVD_PATH}cvd_src/cvd_timer.cc
	${LIBCVD_PATH}cvd_src/deinterlacebuffer.cc
	${LIBCVD_PATH}cvd_src/diskbuffer2.cc
	${LIBCVD_PATH}cvd_src/draw.cc
	${LIBCVD_PATH}cvd_src/exceptions.cc
	${LIBCVD_PATH}cvd_src/faster_corner_utilities.h
	${LIBCVD_PATH}cvd_src/image_io.cc
	${LIBCVD_PATH}cvd_src/morphology.cc
	${LIBCVD_PATH}cvd_src/nonmax_suppression.cxx
	${LIBCVD_PATH}cvd_src/quartic.cpp
	${LIBCVD_PATH}cvd_src/timeddiskbuffer.cc
	${LIBCVD_PATH}cvd_src/videofilebuffer_exceptions.cc
	${LIBCVD_PATH}cvd_src/videosource.cpp
	${LIBCVD_PATH}cvd_src/yuv411_to_stuff.cxx
	${LIBCVD_PATH}cvd_src/yuv420.cpp
	${LIBCVD_PATH}cvd_src/fast_corner.cxx
	${LIBCVD_PATH}cvd_src/fast/fast_corner_9_nonmax.cxx
	${LIBCVD_PATH}cvd_src/fast/fast_10_detect.cxx
	${LIBCVD_PATH}cvd_src/fast/fast_10_score.cxx
	${LIBCVD_PATH}cvd_src/fast/fast_11_detect.cxx
	${LIBCVD_PATH}cvd_src/fast/fast_11_score.cxx
	${LIBCVD_PATH}cvd_src/fast/fast_12_detect.cxx
	${LIBCVD_PATH}cvd_src/fast/fast_12_score.cxx
	${LIBCVD_PATH}cvd_src/fast/fast_7_detect.cxx
	${LIBCVD_PATH}cvd_src/fast/fast_7_score.cxx
	${LIBCVD_PATH}cvd_src/fast/fast_8_detect.cxx
	${LIBCVD_PATH}cvd_src/fast/fast_8_score.cxx
	${LIBCVD_PATH}cvd_src/fast/fast_9_detect.cxx
	${LIBCVD_PATH}cvd_src/fast/fast_9_score.cxx
	${LIBCVD_PATH}cvd_src/image_io/bmp.cxx
	${LIBCVD_PATH}cvd_src/image_io/bmp_read.cc
	${LIBCVD_PATH}cvd_src/image_io/bmp_write.cc
	${LIBCVD_PATH}cvd_src/image_io/cvdimage.cxx
	${LIBCVD_PATH}cvd_src/image_io/fits.cc
	${LIBCVD_PATH}cvd_src/image_io/fitswrite.cc
	${LIBCVD_PATH}cvd_src/image_io/pnm_grok.cxx
	${LIBCVD_PATH}cvd_src/image_io/save_postscript.cxx
	${LIBCVD_PATH}cvd_src/image_io/text.cxx
	${LIBCVD_PATH}cvd_src/image_io/text_write.cc
	${LIBCVD_PATH}cvd_src/noarch/convert_rgb_to_y.cc
	${LIBCVD_PATH}cvd_src/noarch/convolve_gaussian.cc
	${LIBCVD_PATH}cvd_src/noarch/gradient.cc
	${LIBCVD_PATH}cvd_src/noarch/half_sample.cc
	${LIBCVD_PATH}cvd_src/noarch/median_3x3.cc
	${LIBCVD_PATH}cvd_src/noarch/two_thirds_sample.cc
	${LIBCVD_PATH}cvd_src/noarch/utility_byte_differences.cc
	${LIBCVD_PATH}cvd_src/noarch/utility_double_int.cc
	${LIBCVD_PATH}cvd_src/noarch/utility_float.cc
	${LIBCVD_PATH}cvd_src/noarch/yuv422.cpp
	${LIBCVD_PATH}cvd_src/noarch/slower_corner_10.cxx
	${LIBCVD_PATH}cvd_src/noarch/slower_corner_12.cxx
	${LIBCVD_PATH}cvd_src/noarch/slower_corner_9.cxx)
set(HEADERS
    ${LIBCVD_PATH}cvd/argb.h
	${LIBCVD_PATH}cvd/bgrx.h
	${LIBCVD_PATH}cvd/bresenham.h
	${LIBCVD_PATH}cvd/byte.h
	${LIBCVD_PATH}cvd/colourmap.h
	${LIBCVD_PATH}cvd/colourspace.h
	${LIBCVD_PATH}cvd/colourspacebuffer.h
	${LIBCVD_PATH}cvd/colourspaces.h
	${LIBCVD_PATH}cvd/colourspace_convert.h
	${LIBCVD_PATH}cvd/colourspace_frame.h
	${LIBCVD_PATH}cvd/connected_components.h
	${LIBCVD_PATH}cvd/convolution.h
	${LIBCVD_PATH}cvd/deinterlacebuffer.h
	${LIBCVD_PATH}cvd/deinterlaceframe.h
	${LIBCVD_PATH}cvd/diskbuffer2.h
	${LIBCVD_PATH}cvd/diskbuffer2_frame.h
	${LIBCVD_PATH}cvd/distance_transform.h
	${LIBCVD_PATH}cvd/documentation.h
	${LIBCVD_PATH}cvd/draw.h
	${LIBCVD_PATH}cvd/exceptions.h
	${LIBCVD_PATH}cvd/fast_corner.h
	${LIBCVD_PATH}cvd/gles1_helpers.h
	${LIBCVD_PATH}cvd/glwindow.h
	${LIBCVD_PATH}cvd/gl_helpers.h
	${LIBCVD_PATH}cvd/haar.h
	${LIBCVD_PATH}cvd/harris_corner.h
	${LIBCVD_PATH}cvd/helpers.h
	${LIBCVD_PATH}cvd/image.h
	${LIBCVD_PATH}cvd/image_convert.h
	${LIBCVD_PATH}cvd/image_convert_fwd.h
	${LIBCVD_PATH}cvd/image_io.h
	${LIBCVD_PATH}cvd/image_ref.h
	${LIBCVD_PATH}cvd/integral_image.h
	${LIBCVD_PATH}cvd/interpolate.h
	${LIBCVD_PATH}cvd/la.h
	${LIBCVD_PATH}cvd/localvideobuffer.h
	${LIBCVD_PATH}cvd/localvideoframe.h
	${LIBCVD_PATH}cvd/morphology.h
	${LIBCVD_PATH}cvd/nonmax_suppression.h
	${LIBCVD_PATH}cvd/opencv.h
	${LIBCVD_PATH}cvd/rgb.h
	${LIBCVD_PATH}cvd/rgb8.h
	${LIBCVD_PATH}cvd/rgba.h
	${LIBCVD_PATH}cvd/serverpushjpegbuffer.h
	${LIBCVD_PATH}cvd/serverpushjpegframe.h
	${LIBCVD_PATH}cvd/timeddiskbuffer.h
	${LIBCVD_PATH}cvd/timer.h
	${LIBCVD_PATH}cvd/utility.h
	${LIBCVD_PATH}cvd/videobuffer.h
	${LIBCVD_PATH}cvd/videobufferflags.h
	${LIBCVD_PATH}cvd/videobufferwithdata.h
	${LIBCVD_PATH}cvd/videofilebuffer.h
	${LIBCVD_PATH}cvd/videofilebuffer_frame.h
	${LIBCVD_PATH}cvd/videoframe.h
	${LIBCVD_PATH}cvd/videosource.h
	${LIBCVD_PATH}cvd/vision.h
	${LIBCVD_PATH}cvd/vision_exceptions.h
	${LIBCVD_PATH}cvd/yc.h
	${LIBCVD_PATH}cvd/video/skipbuffer.h
	${LIBCVD_PATH}cvd/internal/builtin_components.h
	${LIBCVD_PATH}cvd/internal/convert_pixel_types.h
	${LIBCVD_PATH}cvd/internal/gles1_types.h
	${LIBCVD_PATH}cvd/internal/gl_types.h
	${LIBCVD_PATH}cvd/internal/image_ref_implementation.hh
	${LIBCVD_PATH}cvd/internal/io/bmp.h
	${LIBCVD_PATH}cvd/internal/io/cvdimage.h
	${LIBCVD_PATH}cvd/internal/io/fits.h
	${LIBCVD_PATH}cvd/internal/io/jpeg.h
	${LIBCVD_PATH}cvd/internal/io/parameter.h
	${LIBCVD_PATH}cvd/internal/io/png.h
	${LIBCVD_PATH}cvd/internal/io/pnm_grok.h
	${LIBCVD_PATH}cvd/internal/io/save_postscript.h
	${LIBCVD_PATH}cvd/internal/io/text.h
	${LIBCVD_PATH}cvd/internal/io/tiff.h
	${LIBCVD_PATH}cvd/internal/load_and_save.h
	${LIBCVD_PATH}cvd/internal/name_builtin_types.h
	${LIBCVD_PATH}cvd/internal/name_CVD_rgb_types.h
	${LIBCVD_PATH}cvd/internal/pixel_operations.h
	${LIBCVD_PATH}cvd/internal/pixel_traits.h
	${LIBCVD_PATH}cvd/internal/rgb_components.h
	${LIBCVD_PATH}cvd/internal/scalar_convert.h
	${LIBCVD_PATH}cvd/internal/win.h)


# Library-specific source files, headers and definitions.
if(CVD_dc1394v2_FOUND)
	list(APPEND SRCS
	    ${LIBCVD_PATH}cvd_src/videosource_dvbuffer.cc
	    ${LIBCVD_PATH}cvd_src/Linux/dvbuffer3_dc1394v2.cc)
	list(APPEND HEADERS
	    ${LIBCVD_PATH}cvd/Linux/dvbuffer.h
		${LIBCVD_PATH}cvd/Linux/dvbuffer3.h
		${LIBCVD_PATH}cvd/Linux/dvframe.h)
else()
	list(APPEND SRCS
	    ${LIBCVD_PATH}cvd_src/videosource_nodvbuffer.cc)
endif()

if(OpenGL_OpenGL_FOUND)
	list(APPEND SRCS
	    ${LIBCVD_PATH}cvd_src/gltext.cpp)

	if(X11_FOUND)
		list(APPEND SRCS
		    ${LIBCVD_PATH}cvd_src/glwindow.cc
			${LIBCVD_PATH}cvd_src/videodisplay.cc)
		list(APPEND HEADERS ${LIBCVD_PATH}cvd/videodisplay.h)

		set(CVD_HAVE_GLWINDOW ON)
		set(CVD_HAVE_VIDEODISPLAY ON)
	endif()
endif()

# Platform-specific source files, headers and definitions.
if(WIN32)
	list(APPEND SRCS
	    ${LIBCVD_PATH}cvd_src/Win32/win32.cpp
		${LIBCVD_PATH}cvd_src/Win32/glwindow.cpp)
	set(CVD_HAVE_GLWINDOW ON)
endif()

check_symbol_exists(glob glob.h CVD_HAVE_GLOB)

if(CVD_HAVE_GLOB)
	#list(APPEND SRCS cvd_src/globlist.cxx)
endif()

check_symbol_exists(V4L2_PIX_FMT_GREY linux/videodev2.h CVD_HAVE_V4L)
if(CVD_HAVE_V4L)
	list(APPEND SRCS
	    ${LIBCVD_PATH}cvd_src/Linux/v4lbuffer.cc
		${LIBCVD_PATH}cvd_src/Linux/v4lcontrol.cc
		${LIBCVD_PATH}cvd_src/Linux/videosource_v4lbuffer.cc)
	list(APPEND HEADERS
	    ${LIBCVD_PATH}cvd/Linux/v4lbuffer.h
		${LIBCVD_PATH}cvd/Linux/v4lcontrol.h)
else()
	list(APPEND SRCS ${LIBCVD_PATH}cvd_src/Linux/videosource_nov4lbuffer.cc)
endif()

check_symbol_exists(uvc_stream_get_frame libuvc/libuvc.h CVD_HAVE_UVC)
if(CVD_HAVE_UVC)
	list(APPEND SRCS
	    ${LIBCVD_PATH}cvd_src/uvcbuffer.cc
		${LIBCVD_PATH}cvd_src/videosource_uvcbuffer.cc)
	list(APPEND HEADERS cvd/video/uvcbuffer.h)
else()
	list(APPEND SRCS
	    ${LIBCVD_PATH}cvd_src/videosource_nouvcbuffer.cc)
endif()


if(CVD_FFMPEG_FOUND)
	set(CVD_HAVE_FFMPEG ON)
	list(APPEND SRCS
	    ${LIBCVD_PATH}cvd_src/videofilebuffer2.cc
		${LIBCVD_PATH}cvd_src/videosource_videofilebuffer.cc)
else()
	set(CVD_HAVE_FFMPEG OFF)
	list(APPEND SRCS
	    ${LIBCVD_PATH}cvd_src/videosource_novideofilebuffer.cc)
endif()

if(CVD_TooN_FOUND)
	set(CVD_HAVE_TOON ON)
	list(APPEND SRCS
	    ${LIBCVD_PATH}cvd_src/brezenham.cc
		${LIBCVD_PATH}cvd_src/draw_toon.cc
		${LIBCVD_PATH}cvd_src/tensor_voting.cc
		${LIBCVD_PATH}cvd_src/threepointpose.cpp)
	list(APPEND HEADERS
	    ${LIBCVD_PATH}cvd/brezenham.h
		${LIBCVD_PATH}cvd/camera.h
		${LIBCVD_PATH}cvd/canny.h
		${LIBCVD_PATH}cvd/esm.h
		${LIBCVD_PATH}cvd/image_interpolate.h
		${LIBCVD_PATH}cvd/tensor_voting.h
		${LIBCVD_PATH}cvd/vector_image_ref.h
		${LIBCVD_PATH}cvd/geometry/threepointpose.h)
else()
	set(CVD_HAVE_TOON OFF)
endif()

if(JPEG_FOUND)
	set(CVD_HAVE_JPEG ON)
	set(CVD_INTERNAL_JPEG_BUFFER_SIZE 1)
    list(APPEND SRCS ${LIBCVD_PATH}cvd_src/image_io/jpeg.cxx)
else()
	set(CVD_HAVE_JPEG OFF)
	set(CVD_INTERNAL_JPEG_BUFFER_SIZE OFF)
endif()

if(TIFF_FOUND)
	set(CVD_HAVE_TIFF ON)
    list(APPEND SRCS
	    ${LIBCVD_PATH}cvd_src/image_io/tiff.cxx
		${LIBCVD_PATH}cvd_src/image_io/tiffwrite.cc)
else()
	set(CVD_HAVE_TIFF OFF)
endif()

if(PNG_FOUND)
	set(CVD_HAVE_PNG ON)
    list(APPEND SRCS ${LIBCVD_PATH}cvd_src/image_io/png.cc)
else()
	set(CVD_HAVE_PNG OFF)
endif()

configure_file(${LIBCVD_PATH}cmake/config.h.in ${LIBCVD_PATH}include/cvd/config.h)
configure_file(${LIBCVD_PATH}cmake/config_internal.h.in ${LIBCVD_PATH}include/cvd_src/config_internal.h)

add_library(${PROJECT_NAME} ${SRCS} ${HEADERS})
set_target_properties(${PROJECT_NAME} PROPERTIES OUTPUT_NAME cvd)
target_compile_features(${PROJECT_NAME} PUBLIC cxx_std_14)
target_include_directories(${PROJECT_NAME} PUBLIC "${LIBCVD_PATH}" "${LIBCVD_PATH}/include" ${CVD_DEP_INCLUDES_PUBLIC} PRIVATE ${CVD_DEP_INCLUDES_PRIVATE})
target_link_libraries(${PROJECT_NAME} PRIVATE ${CVD_DEP_LIBS})


if(WIN32)
	target_compile_definitions(${PROJECT_NAME} PRIVATE _CRT_SECURE_NO_WARNINGS)
endif()

install(TARGETS ${PROJECT_NAME} ARCHIVE DESTINATION lib LIBRARY DESTINATION lib)
foreach(file ${HEADERS})
	get_filename_component(dir ${file} DIRECTORY)
	install(FILES ${file} DESTINATION include/${dir})
endforeach()
install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/include/cvd DESTINATION include)
install(DIRECTORY ${PROJECT_SOURCE_DIR}/cmake DESTINATION . FILES_MATCHING PATTERN *.cmake)
