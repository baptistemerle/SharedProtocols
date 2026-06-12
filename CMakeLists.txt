cmake_minimum_required(VERSION 3.16)

set(COMPILER_FLAGS
    $<$<CXX_COMPILER_ID:MSVC>:/W4>
    $<$<NOT:$<CXX_COMPILER_ID:MSVC>>:-Wall>
    $<$<NOT:$<CXX_COMPILER_ID:MSVC>>:-Wextra>
    $<$<NOT:$<CXX_COMPILER_ID:MSVC>>:-Wshadow>
    $<$<NOT:$<CXX_COMPILER_ID:MSVC>>:-Werror=return-type>
    $<$<NOT:$<CXX_COMPILER_ID:MSVC>>:-Wdouble-promotion>
)

if(ESP_PLATFORM)
  idf_component_register(
    INCLUDE_DIRS "src"
  )

  target_compile_options(${COMPONENT_LIB} INTERFACE ${COMPILER_FLAGS})

else()
  project(SharedProtocols LANGUAGES CXX)

  add_library(SharedProtocols INTERFACE)

  target_include_directories(SharedProtocols INTERFACE
    "${CMAKE_CURRENT_SOURCE_DIR}/src"
  )

  target_compile_options(SharedProtocols INTERFACE ${COMPILER_FLAGS})
endif()