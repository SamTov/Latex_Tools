ARTICLE=script
ARTICLE_PATH=.

### -------------- ###
### Function calls ###
### -------------- ###

# Get the OS of the system.
ifeq ($(OS),Windows_NT)
    detected_OS := Windows
else
    detected_OS := $(shell sh -c 'uname 2>/dev/null || echo Unknown')
endif

# Define the standard Latex commend
define tex_command
	pdflatex -interaction nonstopmode -halt-on-error -file-line-error $(1) >> logging.out
endef

# Define the pipeline for latex compile
define compile_pipe
        cd $(ARTICLE_PATH) ; $(call tex_command, $(1)) && bibtex $(1) &&  $(call tex_command, $(1)) && $(call tex_command, $(1))
endef	

# Define some screen output for UI
define begin_out
	@echo Compiling for $(1) system
endef

define end_out
	@echo Compilation complete
endef

# Combine the calls into a single function
define run_call
	$(call begin_out, $(detected_OS))       
        $(call compile_pipe, $(ARTICLE))
        $(call end_out,)
endef

### --------------- ###
### Make components ###
### --------------- ###

.SILENT: linux macos clean  # suppress screen output for bash calls

# Compile and open in linux system
linux:	
	$(call run_call,)
	evince $(ARTICLE_PATH)/$(ARTICLE).pdf

# Compile and open on macos system
macos:
	$(call run_call,)
	open $(ARTICLE_PATH)/$(ARTICLE).pdf

# Clean latex compile directories
clean:
	@echo Cleaning directories
	cd $(ARTICLE_PATH) ; rm *.aux *.bbl *.blg *.log *.pdf logging.out
	clear
	@echo All done!

