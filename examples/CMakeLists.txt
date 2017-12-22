add_executable(one one.cc)
cxx_link(one base dl ${Boost_LIBRARIES})
add_include(one ${Boost_INCLUDE_DIR})
