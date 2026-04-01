cmake_minimum_required(VERSION 3.24)
project(LumeBase)

#
# Target: SecurecAPI
#
add_library(SecurecAPI INTERFACE)
add_library(securec::SecurecAPI ALIAS SecurecAPI)
target_include_directories(SecurecAPI INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/third_party/libboundscheck-1.1.16/include)

#
# Target: securec
#
set(securec_sources
  ${CMAKE_CURRENT_SOURCE_DIR}/third_party/libboundscheck-1.1.16/src/memset_s.c
  ${CMAKE_CURRENT_SOURCE_DIR}/third_party/libboundscheck-1.1.16/src/snprintf_s.c)
add_library(securec STATIC ${securec_sources})
add_library(securec::securec ALIAS securec)
target_link_libraries(securec PRIVATE SecurecAPI)

#
# Target: LumeBaseAPI
#
add_library(LumeBaseAPI INTERFACE)
add_library(LumeBaseAPI::API ALIAS LumeBaseAPI)
add_library(AGPBaseAPI ALIAS LumeBaseAPI)
add_library(AGPBase::AGPBaseAPI ALIAS LumeBaseAPI)
target_include_directories(LumeBaseAPI INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/api)
target_link_libraries(LumeBaseAPI INTERFACE
  securec::SecurecAPI
  securec::securec)
