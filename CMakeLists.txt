cmake_minimum_required (VERSION 3.6)
project (imagemagick)

find_package(ZLIB REQUIRED)
find_package(BZip2 REQUIRED)
find_package(JPEG REQUIRED)
find_package(PNG REQUIRED)
find_package(TIFF REQUIRED)
find_package(Freetype REQUIRED)
#find_package(JBig REQUIRED)

add_definitions(-D_MAGICKLIB_ -D_WANDLIB_ -DMAGICK_IMPLEMENTATION)

if (BUILD_SHARED_LIBS)
    add_definitions(-D_DLL -DDLL)
endif ()

if (MSVC)
    add_definitions( -D_VISUALC_ -D_CRT_SECURE_NO_WARNINGS -D_CRT_SECURE_NO_DEPRECATE -D_CRT_NONSTDC_NO_DEPRECATE)
endif ()

if (WIN32)
    add_definitions(-DWIN32 -D_WIN32)
    add_definitions(-D_WIN32_WINNT=0x0501)
endif ()

include_directories(".")
include_directories("MagickCore")
include_directories("Magick++")
include_directories("Magick++/lib")
include_directories("MagickWand")

file(READ "MagickCore/magick_config.h.in" CONFIG_H)
string(REPLACE "#undef HasBZLIB" "#define HasBZLIB" CONFIG_H "${CONFIG_H}")
string(REPLACE "#undef HasJPEG" "#define HasJPEG" CONFIG_H "${CONFIG_H}")
string(REPLACE "#undef HasPNG" "#define HasPNG" CONFIG_H "${CONFIG_H}")
string(REPLACE "#undef HasTIFF" "#define HasTIFF" CONFIG_H "${CONFIG_H}")
string(REPLACE "#undef HasTTF" "#define HasTTF" CONFIG_H "${CONFIG_H}")
string(REPLACE "#undef HasTTF" "#define HasTTF" CONFIG_H "${CONFIG_H}")
string(REPLACE "#undef HAVE_FT2BUILD_H" "#define HAVE_FT2BUILD_H" CONFIG_H "${CONFIG_H}")
string(REPLACE "#undef HasZLIB" "#define HasZLIB" CONFIG_H "${CONFIG_H}")
string(REPLACE "#undef HasJBIG" "#define HasJBIG" CONFIG_H "${CONFIG_H}")
string(REPLACE "#undef HasWEBP" "#define HasWEBP" CONFIG_H "${CONFIG_H}")
string(REPLACE "#undef QuantumDepth" "#define QuantumDepth 16" CONFIG_H "${CONFIG_H}")
string(REPLACE "#undef restrict" "#define restrict" CONFIG_H "${CONFIG_H}")
string(REPLACE "#undef size_t" "/* #undef size_t */" CONFIG_H "${CONFIG_H}")
if (WIN32)
string(REPLACE "#undef HasWINGDI32" "#define HasWINGDI32" CONFIG_H "${CONFIG_H}")
else ()
string(REPLACE "#undef HAVE_VSNPRINTF" "#define HAVE_VSNPRINTF" CONFIG_H "${CONFIG_H}")
string(REPLACE "#undef MagickLibSubdir" "#define MagickLibSubdir \"magick\"" CONFIG_H "${CONFIG_H}")
string(REPLACE "#undef MagickLibConfigSubDir" "#define MagickLibConfigSubDir \"MagickCore/config\"" CONFIG_H "${CONFIG_H}")
string(REPLACE "#undef MagickShareConfigSubDir" "#define MagickShareConfigSubDir \"MagickCore/config\"" CONFIG_H "${CONFIG_H}")
string(REPLACE "#undef HAVE_STDINT_H" "#define HAVE_STDINT_H" CONFIG_H "${CONFIG_H}")
endif ()
file(WRITE "MagickCore/magick_config.h" "${CONFIG_H}")

#file(READ "MagickCore/magick_config_api.h.in" CONFIG_API_H)
#string(REPLACE "#undef QuantumDepth" "#define QuantumDepth 16" CONFIG_API_H "${CONFIG_API_H}")
#string(REPLACE "#undef size_t" "/* #undef size_t */" CONFIG_API_H "${CONFIG_API_H}")
#file(WRITE "MagickCore/magick_config_api.h" "${CONFIG_API_H}")

add_library(graphicsmagick coders/art.c coders/avs.c
            coders/bmp.c coders/braille.c coders/cals.c coders/caption.c
            coders/cineon.c coders/clipboard.c coders/cmyk.c
            coders/cut.c coders/dcm.c coders/dcraw.c coders/dib.c
            coders/dps.c coders/dpx.c coders/emf.c coders/ept.c
            coders/fax.c coders/fits.c coders/fpx.c coders/gif.c
            coders/gradient.c coders/gray.c coders/histogram.c
            coders/hrz.c coders/html.c coders/icon.c coders/identity.c
            coders/info.c coders/jbig.c coders/jnx.c coders/jp2.c
            coders/jpeg.c coders/label.c coders/locale.c coders/logo.c
            coders/mac.c coders/map.c coders/mat.c coders/matte.c
            coders/meta.c coders/miff.c coders/mono.c coders/mpc.c
            coders/mpeg.c coders/mpr.c coders/msl.c coders/mtv.c
            coders/mvg.c coders/null.c coders/otb.c coders/palm.c
            coders/pcd.c coders/pcl.c coders/pcx.c coders/pdb.c
            coders/pdf.c coders/pict.c coders/pix.c coders/plasma.c
            coders/png.c coders/pnm.c coders/preview.c coders/ps.c
            coders/ps2.c coders/ps3.c coders/psd.c coders/pwp.c coders/rgb.c
            coders/rla.c coders/rle.c coders/sct.c coders/sfw.c coders/sgi.c
            coders/stegano.c coders/sun.c coders/svg.c coders/tga.c
            coders/tiff.c coders/tile.c coders/tim.c coders/topol.c
            coders/ttf.c coders/txt.c coders/uil.c coders/url.c
            coders/uyvy.c coders/vicar.c coders/vid.c coders/viff.c
            coders/wbmp.c coders/webp.c coders/wmf.c coders/wpg.c
            coders/x.c coders/xbm.c coders/xc.c coders/xcf.c
            coders/xpm.c coders/xtrn.c coders/xwd.c coders/yuv.c
            filters/analyze.c
            MagickCore/analyze.c MagickCore/animate.c MagickCore/annotate.c MagickCore/attribute.c
            MagickCore/average.c MagickCore/bit_stream.c MagickCore/blob.c MagickCore/cdl.c
            MagickCore/channel.c MagickCore/color.c MagickCore/color_lookup.c MagickCore/colormap.c
            MagickCore/colorspace.c MagickCore/command.c MagickCore/compare.c MagickCore/composite.c
            MagickCore/compress.c MagickCore/confirm_access.c MagickCore/constitute.c MagickCore/decorate.c
            MagickCore/delegate.c MagickCore/deprecate.c MagickCore/describe.c MagickCore/display.c MagickCore/draw.c
            MagickCore/effect.c MagickCore/enhance.c MagickCore/enum_strings.c MagickCore/error.c MagickCore/export.c
            MagickCore/floats.c MagickCore/fx.c MagickCore/gem.c MagickCore/gradient.c MagickCore/hclut.c
            MagickCore/image.c MagickCore/import.c MagickCore/list.c MagickCore/locale.c MagickCore/log.c
            MagickCore/magic.c MagickCore/magick.c MagickCore/magick_endian.c MagickCore/map.c
            MagickCore/memory.c MagickCore/module.c MagickCore/monitor.c MagickCore/montage.c MagickCore/nt_base.c
            MagickCore/nt_feature.c MagickCore/omp_data_view.c MagickCore/operator.c MagickCore/paint.c
            MagickCore/pixel_cache.c MagickCore/pixel_iterator.c MagickCore/plasma.c MagickCore/PreRvIcccm.c
            MagickCore/profile.c MagickCore/quantize.c MagickCore/random.c MagickCore/registry.c MagickCore/render.c
            MagickCore/resize.c MagickCore/resource.c MagickCore/segment.c MagickCore/semaphore.c
            MagickCore/shear.c MagickCore/signature.c MagickCore/static.c MagickCore/statistics.c
            MagickCore/tempfile.c MagickCore/texture.c MagickCore/timer.c MagickCore/transform.c
            MagickCore/tsd.c MagickCore/type.c MagickCore/unix_port.c MagickCore/utility.c
            MagickCore/version.c MagickCore/widget.c MagickCore/xwindow.c
            Magick++/lib/Blob.cpp Magick++/lib/BlobRef.cpp Magick++/lib/CoderInfo.cpp
            Magick++/lib/Color.cpp Magick++/lib/Drawable.cpp Magick++/lib/Exception.cpp
            Magick++/lib/Functions.cpp Magick++/lib/Geometry.cpp Magick++/lib/Image.cpp
            Magick++/lib/ImageRef.cpp Magick++/lib/Montage.cpp Magick++/lib/Options.cpp
            Magick++/lib/Pixels.cpp Magick++/lib/STL.cpp Magick++/lib/Thread.cpp
            Magick++/lib/TypeMetric.cpp
            MagickWand/drawing_wand.c MagickWand/magick_compat.c MagickWand/magick_wand.c MagickWand/pixel_wand.c)

TARGET_LINK_LIBRARIES(graphicsmagick PRIVATE
    ZLIB::ZLIB
    BZip2::BZip2
    JPEG::JPEG
    PNG::PNG
    TIFF::TIFF
    Freetype::Freetype
    WebP::webp WebP::webpdemux WebP::libwebpmux WebP::webpdecoder
#    JBIG::JBIG
)
target_include_directories(graphicsmagick INTERFACE $<INSTALL_INTERFACE:include>)

install(TARGETS graphicsmagick
    EXPORT graphicsmagick-targets
    RUNTIME DESTINATION bin
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib)

install(
    EXPORT graphicsmagick-targets
    FILE unofficial-graphicsmagick-targets.cmake
    NAMESPACE unofficial::graphicsmagick::
    DESTINATION share/unofficial-graphicsmagick
)
find_package(ZLIB REQUIRED)
find_package(BZip2 REQUIRED)
find_package(JPEG REQUIRED)
find_package(PNG REQUIRED)
find_package(TIFF REQUIRED)
find_package(Freetype REQUIRED)
find_package(WebP CONFIG REQUIRED)
find_package(JBig CONFIG REQUIRED)

file(WRITE ${CMAKE_CURRENT_BINARY_DIR}/unofficial-imagemagick-config.cmake "
include(CMakeFindDependencyMacro)
find_dependency(ZLIB)
find_dependency(BZip2)
find_dependency(JPEG)
find_dependency(PNG)
find_dependency(TIFF)
find_dependency(Freetype)
find_dependency(WebP)
#find_dependency(JBig)

include(\${CMAKE_CURRENT_LIST_DIR}/unofficial-imagemagick-targets.cmake)
")
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/unofficial-imagemagick-config.cmake DESTINATION share/unofficial-imagemagick)

option(INSTALL_HEADERS "Install development header files" ON)

if (INSTALL_HEADERS)
    install(FILES MagickCore/alpha_composite.h DESTINATION include/MagickCore)
    install(FILES MagickCore/analyze.h DESTINATION include/MagickCore)
    install(FILES MagickCore/api.h DESTINATION include/MagickCore)
    install(FILES MagickCore/attribute.h DESTINATION include/MagickCore)
    install(FILES MagickCore/average.h DESTINATION include/MagickCore)
    install(FILES MagickCore/bit_stream.h DESTINATION include/MagickCore)
    install(FILES MagickCore/blob.h DESTINATION include/MagickCore)
    install(FILES MagickCore/cdl.h DESTINATION include/MagickCore)
    install(FILES MagickCore/channel.h DESTINATION include/MagickCore)
    install(FILES MagickCore/color.h DESTINATION include/MagickCore)
    install(FILES MagickCore/colormap.h DESTINATION include/MagickCore)
    install(FILES MagickCore/colorspace.h DESTINATION include/MagickCore)
    install(FILES MagickCore/color_lookup.h DESTINATION include/MagickCore)
    install(FILES MagickCore/command.h DESTINATION include/MagickCore)
    install(FILES MagickCore/common.h DESTINATION include/MagickCore)
    install(FILES MagickCore/compare.h DESTINATION include/MagickCore)
    install(FILES MagickCore/composite.h DESTINATION include/MagickCore)
    install(FILES MagickCore/compress.h DESTINATION include/MagickCore)
    install(FILES MagickCore/confirm_access.h DESTINATION include/MagickCore)
    install(FILES MagickCore/constitute.h DESTINATION include/MagickCore)
    install(FILES MagickCore/decorate.h DESTINATION include/MagickCore)
    install(FILES MagickCore/delegate.h DESTINATION include/MagickCore)
    install(FILES MagickCore/deprecate.h DESTINATION include/MagickCore)
    install(FILES MagickCore/describe.h DESTINATION include/MagickCore)
    install(FILES MagickCore/draw.h DESTINATION include/MagickCore)
    install(FILES MagickCore/effect.h DESTINATION include/MagickCore)
    install(FILES MagickCore/enhance.h DESTINATION include/MagickCore)
    install(FILES MagickCore/enum_strings.h DESTINATION include/MagickCore)
    install(FILES MagickCore/error.h DESTINATION include/MagickCore)
    install(FILES MagickCore/floats.h DESTINATION include/MagickCore)
    install(FILES MagickCore/forward.h DESTINATION include/MagickCore)
    install(FILES MagickCore/fx.h DESTINATION include/MagickCore)
    install(FILES MagickCore/gem.h DESTINATION include/MagickCore)
    install(FILES MagickCore/gradient.h DESTINATION include/MagickCore)
    install(FILES MagickCore/hclut.h DESTINATION include/MagickCore)
    install(FILES MagickCore/image.h DESTINATION include/MagickCore)
    install(FILES MagickCore/list.h DESTINATION include/MagickCore)
    install(FILES MagickCore/locale_c.h DESTINATION include/MagickCore)
    install(FILES MagickCore/log.h DESTINATION include/MagickCore)
    install(FILES MagickCore/magic.h DESTINATION include/MagickCore)
    install(FILES MagickCore/magick.h DESTINATION include/MagickCore)
    install(FILES MagickCore/magick_config.h DESTINATION include/MagickCore)
    install(FILES MagickCore/magick_config_api.h DESTINATION include/MagickCore)
    install(FILES MagickCore/magick_endian.h DESTINATION include/MagickCore)
    install(FILES MagickCore/magick_types.h DESTINATION include/MagickCore)
    install(FILES MagickCore/map.h DESTINATION include/MagickCore)
    install(FILES MagickCore/memory.h DESTINATION include/MagickCore)
    install(FILES MagickCore/module.h DESTINATION include/MagickCore)
    install(FILES MagickCore/module_aliases.h DESTINATION include/MagickCore)
    install(FILES MagickCore/monitor.h DESTINATION include/MagickCore)
    install(FILES MagickCore/montage.h DESTINATION include/MagickCore)
    install(FILES MagickCore/nt_base.h DESTINATION include/MagickCore)
    install(FILES MagickCore/nt_feature.h DESTINATION include/MagickCore)
    install(FILES MagickCore/omp_data_view.h DESTINATION include/MagickCore)
    install(FILES MagickCore/operator.h DESTINATION include/MagickCore)
    install(FILES MagickCore/paint.h DESTINATION include/MagickCore)
    install(FILES MagickCore/pixel_cache.h DESTINATION include/MagickCore)
    install(FILES MagickCore/pixel_iterator.h DESTINATION include/MagickCore)
    install(FILES MagickCore/plasma.h DESTINATION include/MagickCore)
    install(FILES MagickCore/prefetch.h DESTINATION include/MagickCore)
    install(FILES MagickCore/PreRvIcccm.h DESTINATION include/MagickCore)
    install(FILES MagickCore/profile.h DESTINATION include/MagickCore)
    install(FILES MagickCore/quantize.h DESTINATION include/MagickCore)
    install(FILES MagickCore/random-private.h DESTINATION include/MagickCore)
    install(FILES MagickCore/random.h DESTINATION include/MagickCore)
    install(FILES MagickCore/registry.h DESTINATION include/MagickCore)
    install(FILES MagickCore/render.h DESTINATION include/MagickCore)
    install(FILES MagickCore/resize.h DESTINATION include/MagickCore)
    install(FILES MagickCore/resource.h DESTINATION include/MagickCore)
    install(FILES MagickCore/semaphore.h DESTINATION include/MagickCore)
    install(FILES MagickCore/shear.h DESTINATION include/MagickCore)
    install(FILES MagickCore/signature.h DESTINATION include/MagickCore)
    install(FILES MagickCore/spinlock.h DESTINATION include/MagickCore)
    install(FILES MagickCore/static.h DESTINATION include/MagickCore)
    install(FILES MagickCore/statistics.h DESTINATION include/MagickCore)
    install(FILES MagickCore/studio.h DESTINATION include/MagickCore)
    install(FILES MagickCore/symbols.h DESTINATION include/MagickCore)
    install(FILES MagickCore/tempfile.h DESTINATION include/MagickCore)
    install(FILES MagickCore/texture.h DESTINATION include/MagickCore)
    install(FILES MagickCore/timer.h DESTINATION include/MagickCore)
    install(FILES MagickCore/transform.h DESTINATION include/MagickCore)
    install(FILES MagickCore/tsd.h DESTINATION include/MagickCore)
    install(FILES MagickCore/type.h DESTINATION include/MagickCore)
    install(FILES MagickCore/unix_port.h DESTINATION include/MagickCore)
    install(FILES MagickCore/utility.h DESTINATION include/MagickCore)
    install(FILES MagickCore/version.h DESTINATION include/MagickCore)
    install(FILES MagickCore/widget.h DESTINATION include/MagickCore)
    install(FILES MagickCore/xwindow.h DESTINATION include/MagickCore)

    install(FILES Magick++/lib/Magick++.h DESTINATION include)

    install(FILES Magick++/lib/Magick++/Blob.h DESTINATION include/Magick++)
    install(FILES Magick++/lib/Magick++/BlobRef.h DESTINATION include/Magick++)
    install(FILES Magick++/lib/Magick++/CoderInfo.h DESTINATION include/Magick++)
    install(FILES Magick++/lib/Magick++/Color.h DESTINATION include/Magick++)
    install(FILES Magick++/lib/Magick++/Drawable.h DESTINATION include/Magick++)
    install(FILES Magick++/lib/Magick++/Exception.h DESTINATION include/Magick++)
    install(FILES Magick++/lib/Magick++/Functions.h DESTINATION include/Magick++)
    install(FILES Magick++/lib/Magick++/Geometry.h DESTINATION include/Magick++)
    install(FILES Magick++/lib/Magick++/Image.h DESTINATION include/Magick++/)
    install(FILES Magick++/lib/Magick++/ImageRef.h DESTINATION include/Magick++)
    install(FILES Magick++/lib/Magick++/Include.h DESTINATION include/Magick++)
    install(FILES Magick++/lib/Magick++/Montage.h DESTINATION include/Magick++)
    install(FILES Magick++/lib/Magick++/Options.h DESTINATION include/Magick++)
    install(FILES Magick++/lib/Magick++/Pixels.h DESTINATION include/Magick++)
    install(FILES Magick++/lib/Magick++/STL.h DESTINATION include/Magick++)
    install(FILES Magick++/lib/Magick++/Thread.h DESTINATION include/Magick++)
    install(FILES Magick++/lib/Magick++/TypeMetric.h DESTINATION include/Magick++)

    install(FILES MagickWand/drawing_wand.h DESTINATION include/MagickWand)
    install(FILES MagickWand/magick_wand.h DESTINATION include/MagickWand)
    install(FILES MagickWand/pixel_wand.h DESTINATION include/MagickWand)
    install(FILES MagickWand/wand_api.h DESTINATION include/MagickWand)
    install(FILES MagickWand/wand_private.h DESTINATION include/MagickWand)
    install(FILES MagickWand/wand_symbols.h DESTINATION include/MagickWand)
endif ()
