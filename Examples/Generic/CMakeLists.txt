cmake_minimum_required( VERSION 3.23 )

add_executable( BatchLoad BatchLoad.cpp )
target_link_libraries( BatchLoad PRIVATE FreeImage )
set_target_properties( BatchLoad
	PROPERTIES FOLDER Examples
)

add_executable( CloneMultiPage CloneMultiPage.cpp )
target_link_libraries( CloneMultiPage PRIVATE FreeImage )
set_target_properties( CloneMultiPage
	PROPERTIES FOLDER Examples
)

