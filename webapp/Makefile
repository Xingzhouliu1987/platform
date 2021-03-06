.PHONY: build test run clean stop check-style run-unit

BUILD_SERVER_DIR = ..

check-style: .yarninstall
	@echo Checking for style guide compliance

	yarn run check

test: .yarninstall
	cd $(BUILD_SERVER_DIR) && $(MAKE) internal-test-web-client

.yarninstall: package.json
	@echo Getting dependencies using yarn

	yarn cache clean
	yarn install --pure-lockfile

	touch $@

build: .yarninstall
	@echo Building mattermost Webapp

	rm -rf dist

	yarn run build

run: .yarninstall
	@echo Running mattermost Webapp for development

	yarn run run &

run-fullmap: .yarninstall
	@echo FULL SOURCE MAP Running mattermost Webapp for development FULL SOURCE MAP

	yarn run run-fullmap &

stop:
	@echo Stopping changes watching

ifeq ($(OS),Windows_NT)
	wmic process where "Caption='node.exe' and CommandLine like '%webpack%'" call terminate
else
	@for PROCID in $$(ps -ef | grep "[n]ode.*[w]ebpack" | awk '{ print $$2 }'); do \
		echo stopping webpack watch $$PROCID; \
		kill $$PROCID; \
	done
endif

clean:
	@echo Cleaning Webapp

	rm -rf dist
	rm -rf node_modules
	rm -f .yarninstall

	yarn cache clean
