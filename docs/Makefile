################################################################################
#  Makefile for workflows documentation
#
#  Setup
include Makefile_env

#-------------------------------------------------------------------------------

.PHONY: generate_docs
generate_docs: ## Use the Markdown to generate the HTML and PDF documentation
	@echo "🚀 Generating documentation..."
	pandoc --toc=true --pdf-engine=xelatex --quiet \
	-V mainfont="Helvetica" \
	-o '/Volumes/SharedSpace/project docs/Other Computer Projects/workflows/users_guide.pdf' \
	users_guide.md


################################################################################

.PHONY: help
help:
	@uv run --no-sync python -c "import re; \
	[[print(f'\033[36m{m[0]:<20}\033[0m {m[1]}') for m in re.findall(r'^([a-zA-Z_-]+):.*?## (.*)$$', open(makefile).read(), re.M)] for makefile in ('$(MAKEFILE_LIST)').strip().split()]"

.DEFAULT_GOAL := help
