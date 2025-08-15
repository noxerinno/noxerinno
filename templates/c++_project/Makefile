###### Global vars ######
# General
PROG_NAME = my_prog
PROG_VERSION = 0.1.0

# Compiler
CC = g++
FLAGS = -Wall -Wextra -std=c++14 -pedantic

# Directories
SRC_DIR = ./src
MODULES =
BUILD_DIR = ./build
DOC_DIR = ./doc
INCLUDE_DIR = ./include
LIB_DIR = ./lib
TEST_DIR = ./tests

# Builder
CMAKE_LISTS = CMakeLists.txt
CMAKE_MIN_VERSION = 3.0.0
CMAKE_OUTPUT = $(BUILD_DIR)/$(PROG_NAME)

# Files
MAIN = $(SRC_DIR)/Main.cpp
OUTPUT = $(BUILD_DIR)/Main.o
README = README.md

###### FUNCTIONS ###### 
define CAMELCASE
$(shell echo $(1) | sed -E 's/(^| )([a-z])/\U\2/g' | sed 's/ //g' | sed 's/^\(.\)/\L\1/')
endef

define CAPITALIZE_FIRST_LETTER
$(shell echo $(1) | sed 's/^\(.\)/\U\1/')
endef

define CAPITALIZE
$(shell echo $(1) | tr a-z A-Z)
endef

###### Prompt colors & utils ###### 
GREEN = \033[0;32m
RED = \033[0;31m
NO_COLOR = \033[0m
NO_STDOUT = 1>/dev/null
NO_STDERR = 2>/dev/null
NO_PROMPT = $(NO_STDOUT) $(NO_STDERR)



.PHONY: filesystem module help clean

all: filesystem
	@ #$(CC) $(FLAGS) $(MAIN) $(MODULES) -o $(OUTPUT)
	@ #$(OUTPUT)
	@ cd $(BUILD_DIR) && cmake .. $(NO_STDOUT)
	@ cd $(BUILD_DIR) && cmake --build . --parallel $(NO_STDOUT)
	@ echo "${GREEN}CMake: ${NO_COLOR}Build files generated successfully";
	@ $(CMAKE_OUTPUT)


# ==================================================================
# FILESYSTEM
# ==================================================================
## NAME
##     filesystem - Initializes the basic project directory structure and essential files
##
## SYNOPSIS
##     make filesystem
##
## DESCRIPTION
##     This target ensures that all necessary directories and files for a C++ project exist.
##     If they do not exist, they will be automatically created:
##       - Source directory: $(SRC_DIR)
##       - Build directory: $(BUILD_DIR)
##       - Documentation directory: $(DOC_DIR)
##       - Include directory: $(INCLUDE_DIR)
##       - Libraries directory: $(LIB_DIR)
##       - Tests directory: $(TEST_DIR)
##       - Main C++ file: $(MAIN) with a default "Hello, World!" program
##       - CMakeLists.txt file: $(CMAKE_LISTS) with a minimal CMake setup
##       - README.md: $(README) initialized with the project name
##
## NOTES
##     - If a directory or file already exists, it will not be modified.
##     - This target is useful for bootstrapping a new C++ project quickly.
filesystem:	
	@ if [ ! -d $(SRC_DIR) ]; then \
		mkdir $(SRC_DIR); \
		echo "${GREEN}Filesystem: ${NO_COLOR}Source directory created"; \
	fi
	@ if [ ! -d $(BUILD_DIR) ]; then \
		mkdir $(BUILD_DIR); \
		echo "${GREEN}Filesystem: ${NO_COLOR}Build directory created"; \
	fi
	@ if [ ! -d $(DOC_DIR) ]; then \
		mkdir $(DOC_DIR); \
		echo "${GREEN}Filesystem: ${NO_COLOR}Documentation directory created"; \
	fi
	@ if [ ! -d $(INCLUDE_DIR) ]; then \
		mkdir $(INCLUDE_DIR); \
		echo "${GREEN}Filesystem: ${NO_COLOR}Includes directory created"; \
	fi
	@ if [ ! -d $(LIB_DIR) ]; then \
		mkdir $(LIB_DIR); \
		echo "${GREEN}Filesystem: ${NO_COLOR}Libraries directory created"; \
	fi
	@ if [ ! -d $(TEST_DIR) ]; then \
		mkdir $(TEST_DIR); \
		echo "${GREEN}Filesystem: ${NO_COLOR}Tests directory created"; \
	fi
	@ if [ ! -f $(MAIN) ]; then \
		touch $(MAIN); \
		echo "#include <iostream>\n\nint main(){\n\tstd::cout << \"Hello, World!\" << std::endl;\n\treturn 0;\n}" > $(MAIN); \
		echo "${GREEN}Filesystem: ${NO_COLOR}Main file created"; \
	fi
	@ if [ ! -f $(CMAKE_LISTS) ]; then \
		touch $(CMAKE_LISTS); \
		echo "cmake_minimum_required(VERSION $(CMAKE_MIN_VERSION))\nproject($(PROG_NAME) VERSION $(PROG_VERSION))\n\nadd_executable($(PROG_NAME) $(MAIN))" > $(CMAKE_LISTS) ; \
		echo "${GREEN}Filesystem: ${NO_COLOR}CMake file created"; \
	fi
	@ if [ ! -f $(README) ]; then \
		basename "`pwd`" | sed -E "s/_/ /g; s/^(.)/# \U\1/" > $(README); \
		echo "${GREEN}Filesystem: ${NO_COLOR}README file created"; \
	fi

# ==================================================================
# MODULE
# ==================================================================
## NAME
##     module - Creates a new C++ module with default folders and files
##
## SYNOPSIS
##     make module NAME="module_name"
##
## DESCRIPTION
##     This target automatically generates the basic structure of a C++ module:
##       - Creates a subfolder in ./src/ and ./include/ corresponding to the module
##       - Creates a subfolder in $(SRC_DIR) and $(INCLUDE_DIR) corresponding to the module
##       - Generates a header (.hpp) file with a preprocessor guard and class declaration
##       - Generates a source (.cpp) file including the header and using the std namespace
##
## OPTIONS
##     NAME="module_name"
##         Name of the module to create. Multiple words are automatically converted to camelCase.
##         Examples:
##             make module NAME="example"
##             make module NAME="my super module"
##
## NOTES
##     - Created files follow this structure:
##         Header: ./include/moduleName/ModuleName.hpp
##         Source: ./src/moduleName/ModuleName.cpp
##     - The first letter of the class is automatically capitalized.
module: DIR_MODULE_NAME = $(call CAMELCASE,$(NAME))
		MODULE_NAME = $(call CAPITALIZE_FIRST_LETTER,$(DIR_MODULE_NAME))
		HEADER_MODULE_NAME = $(call CAPITALIZE,$(DIR_MODULE_NAME))_HPP;
module:
	@ if [ -z "$(NAME)" ]; then \
		echo "${RED}Module: Module name not provided${NO_COLOR}"; \
		exit 1; \
	fi
 	
	@ mkdir -p $(SRC_DIR)/$(DIR_MODULE_NAME)
	@ mkdir -p $(INCLUDE_DIR)/$(DIR_MODULE_NAME)

	@ echo "#ifndef $(HEADER_MODULE_NAME)\n#define $(HEADER_MODULE_NAME)\n\nclass $(MODULE_NAME) {\npublic:\n\t$(MODULE_NAME)();\n};\n\n$(MODULE_NAME)::$(MODULE_NAME)() {}\n\n#endif\t// $(HEADER_MODULE_NAME)" > $(INCLUDE_DIR)/$(DIR_MODULE_NAME)/$(MODULE_NAME).hpp
	@ echo "#include \".$(INCLUDE_DIR)/$(DIR_MODULE_NAME)/$(MODULE_NAME).hpp\"\n\nusing namespace std;\n" > $(SRC_DIR)/$(DIR_MODULE_NAME)/$(MODULE_NAME).cpp
	
	@ echo "${GREEN}Module: ${NO_COLOR}Module ${DIR_MODULE_NAME} created in ${SRC_DIR} and ${INCLUDE_DIR}"


# ==================================================================
# CLEAN
# ==================================================================
## NAME
##     clean - Removes compiled binaries from the build directory
##
## SYNOPSIS
##     make clean
##
## DESCRIPTION
##     This target deletes all files in the build directory ($(BUILD_DIR)).
##     It is typically used to remove previous compilation outputs and start a fresh build.
##
## NOTES
##     - Only files inside $(BUILD_DIR) are removed; source, include, and documentation directories are untouched.
##     - Useful in combination with 'make all' or 'make module' to ensure a clean rebuild.
clean:
	@ rm -r $(BUILD_DIR)/*
	@ echo "${GREEN}Cleaner: ${NO_COLOR}Binaries cleaned"


# help: TARGET_LIST = $(shell grep -E '^##[[:space:]]+[a-zA-Z0-9_-]+[[:space:]]*-' Makefile | sed -E 's/##[[:space:]]+([a-zA-Z0-9_-]+).*/\1/')
# help:
# 	@ if [ -z $(TARGET) ] || [ ! grep -q $(TARGET) $(TARGET_LIST) ]; then \
# 	    echo "Available targets:"; \
# 	    awk ' \
# 	        /^## NOM/ { nom=$$0; getline; synopsis=$$0; print substr(nom,4) "    " substr(synopsis,4) } \
# 	    ' $(MAKEFILE_LIST); \
# 	else \
# 	    awk -v t="$$TARGET" ' \
# 	        BEGIN{in_target=0} \
# 	        /^##/ { if(in_target){print substr($$0,4)} } \
# 	        new_target: /^[a-zA-Z0-9\-_]+:/ { in_target = ($$0 ~ "^"t":") } \
# 	    ' $(MAKEFILE_LIST); \
# 	fi

# help:
# 	@TARGET="$(TARGET)"; \
# 	if [ -z "$$TARGET" ]; then \
# 		awk ' \
# 			/^## NAME/ { \
# 				getline; split($$0,a," - "); target=a[1]; desc=a[2]; \
# 				getline; getline; split($$0,b,"make "); synopsis="make "b[2]; \
# 				printf "[%s]    %s    %s\n", target, desc, synopsis \
# 			}' $(MAKEFILE_LIST); \
# 	else \
# 		awk -v tgt="$$TARGET" ' \
# 			BEGIN{in_target=0} \
# 			/^## NAME/ { \
# 				getline; split($$0,a," - "); target=a[1]; \
# 				if(target==tgt) in_target=1; else in_target=0 \
# 			} \
# 			in_target && /^##/ { print substr($$0,4) } \
# 		' $(MAKEFILE_LIST) | if grep -q .; then cat; else make help; fi

help:
	@ if [[ ! *"$(TARGET)"* == "$(TARGET_LIST)" ]]; then \
		echo "Good condition"; \
	else \
		echo "Other condition"; \
	fi