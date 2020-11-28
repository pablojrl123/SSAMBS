# SSAMBS.Makefile - Short And Sweet Automatic Make Build System
#
# This make file acts as a simple build system for
# small C++ multi-file projects. Currently,
# it works under Linux.
#
# Created by: P. Rodriguez
#	Last revision: November 28, 2020

#####################################
# Usage:                            #
# 1. Rename this file to 'Makefile' #
# 2. Fill in the information below. #
# 3. Run 'make' in the terminal.    #
#####################################



#######################################
#######################################
# FILL ME
#
# Use only alphanumeric characters and
# no spaces unless otherwise specified.
#
# BIN_DIR and BUILD_DIR will be created
# automatically if they don't exist.
#######################################

# Name of the binary file
PROJECT = SampleProject

# The directories that contain your
# header files, separated by spaces
INCLUDE_DIRS = include

# The root directory of your source
# directory structure
SRC_DIR = src

# The directory to place the binary
BIN_DIR = bin

# Directory to place object files and
# other temporal files used during 
# the build process.
BUILD_DIR = build

# The C++ compiler to use
CXX = g++

# Compiler and linker options
CXXFLAGS = -Wall -Wextra -std=c++11 -g -ggdb #[ADD MORE HERE]
LDFLAGS = #[ADD MORE HERE]

# Tip:
# Use pkg-config or other scripts to
# add compiler/linker flags with:
# CXXFLAGS += $(shell pkg-config --cflags somelibrary)
# LDFLAGS += $(shell pkg-config --libs somelibrary)

#####################################
# END OF FILL ME
#####################################
#####################################








# -------------------------------------- #
# DO NOT CHANGE ANYTHING BELOW THIS LINE #
# -------------------------------------- #

# General explanation (and limitations) of this Makefile
#
# This make file acts as a simple build system for
# small C++ multi-file projects.
#
# Limitations:
#      - Currently, it only works for C++ files (.cpp)
#        (It shouldn't be to difficult to add C capabilities)
#
#      - File and directories names should NOT contain
#        spaces or non-alphanumeric characters.
#
#      - Has been tested only in Ubuntu-based GNU/Linux.
#
# This Makefile traverses the SRC_DIR directory looking
# for .cpp files. Then, it uses the C/C++ compiler
# to generate dependency files (.dep). These files
# contain the header dependencies for each source file.
# Therefore, its corresponding object file will be rebuilt
# if any #include'd header is modified (except for header
# files located in system directories).
#
# Both the object files and dependency files are placed
# in the BUILD_DIR directory in the same structure as the
# SRC_DIR directory. For example, if the SRC_DIR is:
#      SRC_DIR
#      |
#      |- main.cpp
#      |
#      |- more
#            |
#            |- extra.cpp
#
# Then, the build dir is going to look like:
#      BUILD_DIR
#      |
#      |- main.cpp.o
#      |- main.cpp.dep
#      |
#      |- more
#            |
#            |- extra.cpp.o
#            |- extra.cpp.dep
#
# For more information about Make and Makefiles,
# consult the Make manual pages:
# https://www.gnu.org/software/make/manual/make.html
#
# This Makefile uses Static Pattern Rules,
# Substitution References, and Automatic
# Variables all over the place.
# Info:
# http://www.gnu.org/software/make/manual/make.html#Static-Usage
# http://www.gnu.org/software/make/manual/make.html#Substitution-Refs
# http://www.gnu.org/software/make/manual/make.html#Automatic-Variables


# Recursive wildcard to traverse directory structures.
# Usage:
#        $(call recursive_wildcard,rootdir/,*.abc)
# Where rootdir is the top-most directory, and *.abc
# are the files to search. '/' after rootdir is required.
# Taken from LightStruck's answer in StackOverflow:
# https:///stackoverflow.com/a/12959764
recursive_wildcard=$(wildcard $1$2) $(foreach d,$(wildcard $1*),$(call recursive_wildcard,$d/,$2))

SOURCES = $(call recursive_wildcard,$(SRC_DIR)/,*.cpp)
OBJECTS = $(SOURCES:$(SRC_DIR)/%=$(BUILD_DIR)/%.o) # Append ".o" to all sources
DEPENDENCIES = $(SOURCES:$(SRC_DIR)/%=$(BUILD_DIR)/%.dep) # Append ".dep" to all sources
EXECUTABLE = $(BIN_DIR)/$(PROJECT)
I_FLAGS = $(addprefix -I,$(INCLUDE_DIRS)) # Prepend "-I" to every include directory.

# First rule is the default rule.
.PHONY : all
all : $(EXECUTABLE)

# Link all object files into the executable.
$(EXECUTABLE) : $(OBJECTS)
	@mkdir -p $(dir $@)
	$(CXX) $^ $(LDFLAGS) -o $@

# Include the dependency files.
# Each dependency file contains only one
# Make rule. The target of this rule is the
# object file. The inclusion of the dependency
# file before the compile rule is used to give
# extra prerequisites to the object file.
# More information about multiple rules for one
# target and extra prerequisites:
#  https://www.gnu.org/software/make/manual/html_node/Multiple-Rules.html
-include $(DEPENDENCIES)

# Compile the source files into object files.
# Added to the prerequisites are the header
# files, provided by the dependency file.
# (See above)
$(OBJECTS) : $(BUILD_DIR)/%.o : $(SRC_DIR)/% $(BUILD_DIR)/%.dep
	@mkdir -p $(dir $@)
	$(CXX) $(CXXFLAGS) $(I_FLAGS) -o $@ -c $<

# Create the dependency files.
$(DEPENDENCIES) : $(BUILD_DIR)/%.dep : $(SRC_DIR)/%
	@mkdir -p $(dir $@)
	$(CXX) $(CXXFLAGS) $(I_FLAGS) -MM -MT $(@:%.dep=%.o) -MF $@ $<

.PHONY : clean
clean :
	rm -f $(DEPENDENCIES) $(OBJECTS) $(EXECUTABLE)

# Print variables
.PHONY : varinfo
varinfo :
	@echo "CXX=$(CXX)"
	@echo "INCLUDE_DIRS=$(INCLUDE_DIRS)"
	@echo "SRC_DIR=$(SRC_DIR)"
	@echo "BIN_DIR=$(BIN_DIR)"
	@echo "BUILD_DIR=$(BUILD_DIR)"
	@echo "PROJECT=$(PROJECT)"
	@echo "CXXFLAGS=$(CXXFLAGS)"
	@echo "LDFLAGS=$(LDFLAGS)"
	@echo "SOURCES=$(SOURCES)"
	@echo "OBJECTS=$(OBJECTS)"
	@echo "DEPENDENCIES=$(DEPENDENCIES)"
	@echo "EXECUTABLE=$(EXECUTABLE)"
	@echo "I_FLAGS=$(I_FLAGS)"
