
add_subdirectory(capstone2llvmir)
if(CAPSTONE2LLVMIR_TOOLS)
	add_subdirectory(capstone2llvmirtool)
endif()
if(CAPSTONE2LLVMIR_TESTS)
	add_subdirectory(llvmir-emul)
endif()
