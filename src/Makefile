all:
	/usr/bin/python3 ../scripts/update_version.py
	bash run-xcodegen.sh
	xcodebuild -configuration Release -alltargets SYMROOT="$(CURDIR)/build"
# Copy embedded.provisionprofile
	cp ShowyEdge/ShowyEdge.provisionprofile build/Release/ShowyEdge.app/Contents/embedded.provisionprofile
	$(MAKE) codesign
	$(MAKE) -C .. clean-launch-services-database

clean: purge-swift-package-manager-cache
	rm -rf ShowyEdge.xcodeproj
	rm -rf build

codesign:
# Sparkle
	bash ../scripts/codesign.sh build/Release/ShowyEdge.app/Contents/Frameworks/Sparkle.framework/Versions/B/XPCServices/Installer.xpc
	bash ../scripts/codesign.sh build/Release/ShowyEdge.app/Contents/Frameworks/Sparkle.framework/Versions/B/XPCServices/Downloader.xpc $(CURDIR)/org.sparkle-project.Downloader.entitlements
	bash ../scripts/codesign.sh build/Release/ShowyEdge.app/Contents/Frameworks/Sparkle.framework/Versions/B/Autoupdate
	bash ../scripts/codesign.sh build/Release/ShowyEdge.app/Contents/Frameworks/Sparkle.framework/Versions/B/Updater.app
	bash ../scripts/codesign.sh build/Release/ShowyEdge.app/Contents/Frameworks/Sparkle.framework
# ShowyEdge
	bash ../scripts/codesign.sh build/Release/ShowyEdge.app $(CURDIR)/ShowyEdge/ShowyEdge.entitlements

purge-swift-package-manager-cache:
	rm -rf ~/Library/Developer/Xcode/DerivedData/ShowyEdge-*
	rm -rf ~/Library/Caches/org.swift.swiftpm/repositories/Sparkle-*

xcode:
	open ShowyEdge.xcodeproj

run:
	open build/Release/ShowyEdge.app

swift-format:
	find . -name '*.swift' -print0 | xargs -0 swift-format -i

install:
	rsync -a --delete build/Release/ShowyEdge.app /Applications
