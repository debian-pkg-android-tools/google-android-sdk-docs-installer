PKG_SOURCE:=docs-23_r01.zip
PKG_SOURCE_URL:=${DL_MIRROR}/android/repository/${PKG_SOURCE}

DL_DIR=/var/cache/google-android-sdk-docs-installer
UNPACK_DIR=$(DL_DIR)/docs
INSTALL_DIR=/usr/share/doc/google-android-sdk-docs
FILELIST=/var/lib/dpkg/info/google-android-sdk-docs-installer.list

all: $(DL_DIR)/$(UNPACK_DIR)/index.html
	find $(UNPACK_DIR)/ -name '*.js' -o -name '*.html' -o -name '*.css' | xargs sed -i \
		-e 's|http://source.android.com|https://source.android.com|g' \
		-e 's|http://developer.android.com|https://developer.android.com|g' \
		-e 's|http://developer.chrome.com|https://developer.chrome.com|g' \
		-e 's|http://www.youtube.com|https://www.youtube.com|g' \
		-e 's|http://youtu.be|https://youtu.be|g' \
		-e 's|http://i1.ytimg.com|https://i1.ytimg.com|g' \
		-e 's|http://developers.google.com|https://developers.google.com|g' \
		-e 's|http://play.google.com|https://play.google.com|g' \
		-e 's|http://plus.google.com|https://plus.google.com|g' \
		-e 's|http://www.google.com|https://www.google.com|g' \
		-e 's|http://support.google.com|https://support.google.com|g' \
		-e 's|http://android-developers.blogspot|https://android-developers.blogspot|g' \
		-e 's|http://chart.googleapis.com|https://chart.googleapis.com|g' \
		-e 's|http://en.wikipedia.org|https://en.wikipedia.org|g' \
		-e 's|http://creativecommons.org|https://creativecommons.org|g' \
		-e 's|http://fonts\.googleapis\.com/css.family=Roboto+Condensed|./font-family-roboto-condensed.css|g' \
		-e 's|http://fonts\.googleapis\.com/css.family=Roboto:light,regular,medium,thin,italic,mediumitalic,bold|./font-family-roboto-light-regular-medium-thin-italic-mediumitalic-bold.css|g' \
		-e 's|href="../../../guide/topics/fundamentals|href="../../../guide/components|g'

	find $(UNPACK_DIR)/ -name '*.html' | xargs sed -i \
		-e 's|//ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js|./swfobject.js|g' \
		-e 's|<script src="https://www.google.com/jsapi" type="text/javascript"></script>||g' \
		-e 's/^  var _gaq = _gaq || \[\];$$/  var _gaq = [];/' \
		-e "/^  _gaq\.push(\['_setAccount', 'UA-5831155-1'\]);/,/^  \})();/d" \
		-e '/^<!-- Start of Tag -->/,/^<!-- End of Tag -->/d'
	find $(UNPACK_DIR)/ | xargs chmod a+r
	find $(UNPACK_DIR)/ -type d | xargs chmod 0755

install: all
	install -d -m0755 $(INSTALL_DIR)
	install -m0644 $(UNPACK_DIR)/source.properties $(INSTALL_DIR)/
	mv $(UNPACK_DIR) $(INSTALL_DIR)/
	echo $(INSTALL_DIR)/source.properties >> $(FILELIST)
	find $(INSTALL_DIR)/docs >> $(FILELIST)

uninstall:
	rm -f $(INSTALL_DIR)/copyright.gz
	for d in `find $(INSTALL_DIR) /usr/share/doc -type d -empty`; do \
		test -d $$d && rmdir $$d || true ; done

$(DL_DIR)/$(UNPACK_DIR)/index.html: $(DL_DIR)/$(PKG_SOURCE)
	cd $(DL_DIR) && unzip -ou $(PKG_SOURCE)

$(DL_DIR)/$(PKG_SOURCE): $(DL_DIR)
	cd $(DL_DIR) && \
		su nobody -s /bin/sh -c "wget --continue $(PKG_SOURCE_URL)"
	sha1sum -c $(PKG_SOURCE).sha1

$(DL_DIR)/swfobject.js: $(DL_DIR)
	wget --continue 'https://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js' \
		--output-document=$(DL_DIR)/swfobject.js

$(DL_DIR):
	install -d -m0700 $(DL_DIR)

clean:
	rm -rf -- $(UNPACK_DIR)
	rm -f $(DL_DIR)/swfobject.js

distclean: clean
	-rm -rf -- $(DL_DIR)

.PHONY: install clean
