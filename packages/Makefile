# Parent Makefile to call 'make install' and 'make uninstall' in subdirectories, with colored messages

# ANSI color codes
GREEN  := \033[0;32m
RED  := \033[0;31m
YELLOW := \033[1;33m
CYAN   := \033[0;36m
RESET  := \033[0m

# Find all immediate subdirectories
CHILD_DIRS := $(patsubst %/,%,$(shell find * -maxdepth 0 -type d))

.PHONY: all build install uninstall clean

all:
	@echo "$(CYAN)--- No default target specified. Use 'make build', 'make install', 'make uninstall', or 'make clean'. ---$(RESET)"

# Define a generic function for processing subdirectories
# $(1): target name (e.g., build, install)
# $(2): Start/Info color (e.g., CYAN)
# $(3): Reset color (e.g., RESET)
# $(4): Warning/Skip color (e.g., YELLOW)
# $(5): Success color (e.g., GREEN)
# $(6): Failure color (e.g., RED)
define process_subdir_target
	@echo "$($(2))--- Starting $(1) process for all subdirectories ---$($(3))"
	@$(1)_FAILED=0; \
	if [ -z "$(CHILD_DIRS)" ]; then \
		echo "$($(4))No subdirectories found.$($(3))"; \
	else \
		for dir in $(CHILD_DIRS); do \
			if [ -f "$$dir/Makefile" ] || [ -f "$$dir/makefile" ]; then \
				echo ""; \
				echo "$($(2))>>> Processing $(1) in '$$dir' <<<$($(3))"; \
				_SUB_MAKE_CMD_BASE="$(MAKE) -C "$$dir""; \
				_TARGET_ARG_FOR_SUB_MAKE=$(1); \
				_CMD_DESC_FOR_MSG="make $(1)"; \
				if [ "$(1)" = "build" ]; then \
					_TARGET_ARG_FOR_SUB_MAKE=""; \
					_CMD_DESC_FOR_MSG="make (default target)"; \
				fi; \
				echo "$($(2))Running: $$_SUB_MAKE_CMD_BASE $$_TARGET_ARG_FOR_SUB_MAKE$($(3))"; \
				if $$_SUB_MAKE_CMD_BASE $$_TARGET_ARG_FOR_SUB_MAKE; then \
					echo "$($(5))>>> Successfully ran $$_CMD_DESC_FOR_MSG in '$$dir' <<<$($(3))"; \
				else \
					echo "$($(6))>>> $$_CMD_DESC_FOR_MSG FAILED in '$$dir'. <<<$($(3))"; \
					$(1)_FAILED=1; \
				fi; \
			else \
				echo ""; \
				echo "$($(4))>>> No Makefile found in '$$dir', skipping. <<<$($(3))"; \
			fi; \
		done; \
	fi; \
	echo ""; \
	if [ "$$$(1)_FAILED" -eq 1 ]; then \
		echo "$($(6))--- One or more subdirectory $(1) operations FAILED. ---$($(3))"; \
		exit 1; \
	else \
		echo "$($(5))--- All subdirectory $(1) operations completed successfully (or were skipped). ---$($(3))"; \
	fi
endef

# Use the generic function for each target
build:
	$(call process_subdir_target,build,CYAN,RESET,YELLOW,GREEN,RED)

install:
	$(call process_subdir_target,install,CYAN,RESET,YELLOW,GREEN,RED)

uninstall:
	$(call process_subdir_target,uninstall,CYAN,RESET,YELLOW,GREEN,RED)

clean:
	$(call process_subdir_target,clean,CYAN,RESET,YELLOW,GREEN,RED)