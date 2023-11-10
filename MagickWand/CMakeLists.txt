add_definitions(-D_CRT_SECURE_NO_WARNINGS)
add_definitions(-D_LIB)
add_definitions(-D_VISUALC_)
add_definitions(-DNeedFunctionPrototypes)
add_definitions(-D_MAGICKLIB_)
add_definitions(-D_CRT_NONSTDC_NO_WARNINGS)

file(GLOB SRC "${PROJECT_SOURCE_DIR}/*.c")
ADD_LIBRARY(CORE_wand STATIC ${SRC} )

set_target_properties(CORE_wand PROPERTIES COMPILE_FLAGS "/EHsc /GF")

set_target_properties(CORE_wand PROPERTIES
        OUTPUT_NAME CORE_wand
        DEBUG_POSTFIX "d"
    )
#set_target_properties(ZLib PROPERTIES COMPILE_FLAGS "/GF")  #VERY IMPORTANT: can not use twice.

#set_source_files_properties(${ZLIB_SRC} PROPERTIES COMPILE_FLAGS "/EHsc")   #every file compile option.

set(CompilerFlags
        CMAKE_CXX_FLAGS
        CMAKE_CXX_FLAGS_DEBUG
        CMAKE_CXX_FLAGS_RELEASE
        CMAKE_C_FLAGS
        CMAKE_C_FLAGS_DEBUG
        CMAKE_C_FLAGS_RELEASE
        )
foreach(CompilerFlag ${CompilerFlags})
  string(REPLACE "/MD" "/MT" ${CompilerFlag} "${${CompilerFlag}}")
  string(REPLACE "/Ob0" "" ${CompilerFlag} "${${CompilerFlag}}")
  string(REPLACE "/TC" "" ${CompilerFlag} "${${CompilerFlag}}")
endforeach()

INSTALL(TARGETS CORE_wand
RUNTIME DESTINATION bin
LIBRARY DESTINATION lib
ARCHIVE DESTINATION lib
)
