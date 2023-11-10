cmake_minimum_required(VERSION 3.16)

PROJECT(ImageMagick)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

add_subdirectory(Magick++)
add_subdirectory(MagickCore)
add_subdirectory(MagickWand)
add_subdirectory(coders)
add_subdirectory(filters)

file(COPY  "${CMAKE_CURRENT_SOURCE_DIR}/Magick++/lib/Magick++.h"  
DESTINATION  "${CMAKE_BINARY_DIR}/include"  )
install(FILES "${CMAKE_CURRENT_SOURCE_DIR}/Magick++/lib/Magick++.h"
    DESTINATION "include"
    )

# wand

file(GLOB wand_hdrs "${CMAKE_CURRENT_SOURCE_DIR}/wand/*.h")
file(COPY  ${wand_hdrs}
DESTINATION  "${CMAKE_BINARY_DIR}/include/wand"  )

install(FILES ${wand_hdrs}
    DESTINATION "include/wand"
    )


file(COPY  "${CMAKE_CURRENT_SOURCE_DIR}/VisualMagick/wand/wand-config.h.in"  
DESTINATION  "${CMAKE_BINARY_DIR}/include/wand"  )

file(RENAME "${CMAKE_BINARY_DIR}/include/wand/wand-config.h.in" "${CMAKE_BINARY_DIR}/include/wand/wand-config.h")


install(FILES "${CMAKE_CURRENT_SOURCE_DIR}/VisualMagick/wand/wand-config.h.in"
    DESTINATION "include/wand"
      RENAME "wand-config.h")

# Magick++

file(GLOB Magick_hdrs "${CMAKE_CURRENT_SOURCE_DIR}/Magick++/lib/Magick++/*.h")
file(COPY  ${Magick_hdrs}  
DESTINATION  "${CMAKE_BINARY_DIR}/include/Magick++"  )

install(FILES ${Magick_hdrs}
    DESTINATION "include/Magick++"
   )

# magick

file(GLOB magick_hdrs "${CMAKE_CURRENT_SOURCE_DIR}/magick/*.h")
file(COPY ${magick_hdrs}
DESTINATION  "${CMAKE_BINARY_DIR}/include/magick"  
)
install(FILES ${magick_hdrs}
    DESTINATION "include/magick"
   )




