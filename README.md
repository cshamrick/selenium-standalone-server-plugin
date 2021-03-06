Selenium driver-binary-downloader-maven-plugin
=================================

[![Join the chat at https://gitter.im/Ardesco/selenium-standalone-server-plugin](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/Ardesco/selenium-standalone-server-plugin?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![Build Status](https://travis-ci.org/Ardesco/selenium-standalone-server-plugin.svg?branch=master)](https://travis-ci.org/Ardesco/selenium-standalone-server-plugin)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.lazerycode.selenium/driver-binary-downloader-maven-plugin/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.lazerycode.selenium/driver-binary-downloader-maven-plugin)
[![Javadoc](https://javadoc-emblem.rhcloud.com/doc/com.lazerycode.selenium/driver-binary-downloader-maven-plugin/badge.svg)](http://www.javadoc.io/doc/com.lazerycode.selenium/driver-binary-downloader-maven-plugin)


A Maven plugin that will download the WebDriver stand alone server binaries for use in your mavenised Selenium project.

What's changed?  See the [Changelog](https://github.com/Ardesco/selenium-standalone-server-plugin/blob/master/CHANGELOG.md).

**This plugin now requires Java 8!**

Default Usage
-----

    <plugins>
        <plugin>
            <groupId>com.lazerycode.selenium</groupId>
            <artifactId>driver-binary-downloader-maven-plugin</artifactId>
            <version>1.0.17</version>
            <configuration>
                <!-- root directory that downloaded driver binaries will be stored in -->
                <rootStandaloneServerDirectory>/my/location/binaries</rootStandaloneServerDirectory>
                <!-- Where you want to store downloaded zip files -->
                <downloadedZipFileDirectory>/my/location/zips</downloadedZipFileDirectory>
            </configuration>
            <executions>
                <execution>
                    <goals>
                        <goal>selenium</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>

By default the plugin will download the most recent binary specified in the RepositoryMap.xml for every driver/os/bitrate.
If you want to filter out the ones you don't need have a look at the advanced usage options below.

Advanced Usage
-----

    <build>
        <plugins>
            <plugin>
                <groupId>com.lazerycode.selenium</groupId>
                <artifactId>driver-binary-downloader-maven-plugin</artifactId>
                <version>1.0.17</version>
                <configuration>
                    <!-- root directory that downloaded driver binaries will be stored in -->
                    <rootStandaloneServerDirectory>/tmp/binaries</rootStandaloneServerDirectory>
                    <!-- Where you want to store downloaded zip files -->
                    <downloadedZipFileDirectory>/tmp/zips</downloadedZipFileDirectory>
                    <!-- Location of a custom repository map -->
                    <customRepositoryMap>/tmp/repo.xml</customRepositoryMap>
                    <!-- This will ensure that the plugin only downloads binaries for the current OS, this will override anything specified in the <operatingSystems> configuration -->
                    <onlyGetDriversForHostOperatingSystem>false</onlyGetDriversForHostOperatingSystem>
                    <!-- Operating systems you want to download binaries for (Only valid options are: windows, linux, osx) -->
                    <operatingSystems>
                        <windows>true</windows>
                        <linux>true</linux>
                        <mac>true</mac>
                    </operatingSystems>
                    <!-- Download 32bit binaries -->
                    <thirtyTwoBitBinaries>true</thirtyTwoBitBinaries>
                    <!-- Download 64bit binaries -->
                    <sixtyFourBitBinaries>true</sixtyFourBitBinaries>
                    <!-- If set to false will download every version available (Other filters will be taken into account -->
                    <onlyGetLatestVersions>false</onlyGetLatestVersions>
                    <!-- Provide a list of drivers and binary versions to download (this is a map so only one version can be specified per driver) -->
                    <getSpecificExecutableVersions>
                        <googlechrome>18</googlechrome>
                    </getSpecificExecutableVersions>
                    <!-- Throw an exception if any specified binary versions that the plugin tries to download do not exist -->
                    <throwExceptionIfSpecifiedVersionIsNotFound>false</throwExceptionIfSpecifiedVersionIsNotFound>
                    <!-- Number of times to attempt to download each file -->
                    <fileDownloadRetryAttempts>2</fileDownloadRetryAttempts>
                    <!-- Number of ms to wait before timing out when trying to connect to remote server to download file -->
                    <fileDownloadConnectTimeout>20000</fileDownloadConnectTimeout>
                    <!-- Number of ms to wait before timing out when trying to read file from remote server -->
                    <fileDownloadReadTimeout>10000</fileDownloadReadTimeout>
                    <!-- Overwrite any existing binaries that have been downloaded and extracted -->
                    <overwriteFilesThatExist>true</overwriteFilesThatExist>
                    <!-- Check file hashes of downloaded files.  Default: true -->
                    <checkFileHashes>true</checkFileHashes>
                    <!-- auto detect system proxy to use when downloading files -->
                    <!-- To specify an explicit proxy set the environment variables http.proxyHost and http.proxyPort -->
                    <useSystemProxy>true</useSystemProxy>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <goal>selenium</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>

Custom RepositoryMap.xml
-----

You __should__ supply your own RepositoryMap.xml file, if you don't supply one a default one (which will most likely be out of date) will be used instead.  Your RepositoryMap.xml must match the schema available at [Here](https://github.com/Ardesco/selenium-standalone-server-plugin/blob/master/src/main/resources/RepositoryMap.xsd).

How do I get the SHA1/MD5 hases for the binaries I ant to download?

The people providing the binaries should be publishing MD5/SHA1 hashes as well so that you can check that the file you have downloaded is not corrupt.  It seems that recently this does not seem to be happening as a matter of course.  If you can't find a published SHA1/MD5 hash you can always download the file yourself, check that it is not corrupt and then generate the hash using the following commands:

On a *nix system it's pretty easy. perform the following command:

    openssl md5 <filename>
    openssl sha1 <filename>

On windows you can do the following (according to https://support.microsoft.com/en-us/kb/889768):

    FCIV -sha1 <filename>
    FCIV -md5 <filename>

___Below is an example RepositoryMap.xml that I will endeavour to keep up to date so that you can just copy/paste the contents into your own file.___

    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <root>
        <windows>
            <driver id="internetexplorer">
                <version id="3.7.0">
                    <bitrate sixtyfourbit="true">
                        <filelocation>http://selenium-release.storage.googleapis.com/3.7/IEDriverServer_x64_3.7.0.zip</filelocation>
                        <hash>f6a0d7939355acba00f7b1374bebda50bbead59b</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                    <bitrate thirtytwobit="true">
                        <filelocation>http://selenium-release.storage.googleapis.com/3.7/IEDriverServer_Win32_3.7.0.zip</filelocation>
                        <hash>ded0a82cab46538751ba17206f55c112674585d6</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                </version>
            </driver>
            <driver id="edge">
                <version id="5.16299">
                    <bitrate sixtyfourbit="true" thirtytwobit="true">
                        <filelocation>https://download.microsoft.com/download/D/4/1/D417998A-58EE-4EFE-A7CC-39EF9E020768/MicrosoftWebDriver.exe</filelocation>
                        <hash>60c4b6d859ee868ba5aa29c1e5bfa892358e3f96</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                </version>
            </driver>
            <driver id="googlechrome">
                <version id="2.33">
                    <bitrate thirtytwobit="true" sixtyfourbit="true">
                        <filelocation>https://chromedriver.storage.googleapis.com/2.33/chromedriver_win32.zip</filelocation>
                        <hash>b52ca785707eade99c56309ce9d66c1177b5d4a9</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                </version>
            </driver>
            <driver id="operachromium">
                <version id="2.30">
                    <bitrate sixtyfourbit="true">
                        <filelocation>https://github.com/operasoftware/operachromiumdriver/releases/download/v.2.30/operadriver_win64.zip</filelocation>
                        <hash>bf80617b01708ced97f7cc174e72a1b6953ae126</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                    <bitrate thirtytwobit="true">
                        <filelocation>https://github.com/operasoftware/operachromiumdriver/releases/download/v.2.30/operadriver_win32.zip</filelocation>
                        <hash>151dc7ae316be2ccc52c28395823c778ccfda5ac</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                </version>
            </driver>
            <driver id="phantomjs">
                <version id="1.9.8">
                    <bitrate thirtytwobit="true" sixtyfourbit="true">
                        <filelocation>https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-1.9.8-windows.zip</filelocation>
                        <hash>4531bd64df101a689ac7ac7f3e11bb7e77af8eff</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                </version>
            </driver>
            <driver id="marionette">
                <version id="0.19.1">
                    <bitrate sixtyfourbit="true">
                        <filelocation>https://github.com/mozilla/geckodriver/releases/download/v0.19.1/geckodriver-v0.19.1-win64.zip</filelocation>
                        <hash>1d6a7ade773f98b3b5a15ace3b6a06b1ad97a8f1</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                    <bitrate thirtytwobit="true">
                        <filelocation>https://github.com/mozilla/geckodriver/releases/download/v0.19.1/geckodriver-v0.19.1-win32.zip</filelocation>
                        <hash>74258f5c73a8fce66fbc3ecd622dae890f1e475a</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                </version>
            </driver>
        </windows>
        <linux>
            <driver id="googlechrome">
                <version id="2.33">
                    <bitrate sixtyfourbit="true">
                        <filelocation>https://chromedriver.storage.googleapis.com/2.33/chromedriver_linux64.zip</filelocation>
                        <hash>717d67ab192b1c57819528161557ce2b66b9436c</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                    <bitrate thirtytwobit="true">
                        <filelocation>https://chromedriver.storage.googleapis.com/2.33/chromedriver_linux32.zip</filelocation>
                        <hash>cca39f74c1ddd2f2d7afd36f41a9f0db7be88dcf</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                </version>
            </driver>
            <driver id="operachromium">
                <version id="2.30">
                    <bitrate sixtyfourbit="true">
                        <filelocation>https://github.com/operasoftware/operachromiumdriver/releases/download/v.2.30/operadriver_linux64.zip</filelocation>
                        <hash>a8528276dab36e5daa6213db16ac4ae227e8394d</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                </version>
            </driver>
            <driver id="phantomjs">
                <version id="1.9.8">
                    <bitrate sixtyfourbit="true">
                        <filelocation>https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-1.9.8-linux-x86_64.tar.bz2</filelocation>
                        <hash>d29487b2701bcbe3c0a52bc176247ceda4d09d2d</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                    <bitrate thirtytwobit="true">
                        <filelocation>https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-1.9.8-linux-i686.tar.bz2</filelocation>
                        <hash>efac5ae5b84a4b2b3fa845e8390fca39e6e637f2</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                </version>
            </driver>
            <driver id="marionette">
                <version id="0.19.1">
                    <bitrate sixtyfourbit="true">
                        <filelocation>https://github.com/mozilla/geckodriver/releases/download/v0.19.1/geckodriver-v0.19.1-linux64.tar.gz</filelocation>
                        <hash>9284c82e1a6814ea2a63841cd532d69b87eb0d6e</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                    <bitrate thirtytwobit="true">
                        <filelocation>https://github.com/mozilla/geckodriver/releases/download/v0.19.1/geckodriver-v0.19.1-linux32.tar.gz</filelocation>
                        <hash>9375768a70365d361029f050d73e03aba0ece351</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                    <bitrate arm="true">
                        <filelocation>https://github.com/mozilla/geckodriver/releases/download/v0.19.1/geckodriver-v0.19.1-arm7hf.tar.gz</filelocation>
                        <hash>115ad6e36c03445fed1499112156472eaa5faa1d</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                </version>
            </driver>
        </linux>
        <osx>
            <driver id="googlechrome">
                <version id="2.33">
                    <bitrate sixtyfourbit="true">
                        <filelocation>https://chromedriver.storage.googleapis.com/2.33/chromedriver_mac64.zip</filelocation>
                        <hash>eb28e6d716b04bfede98fbb74bae041d6a415aea</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                </version>
            </driver>
            <driver id="operachromium">
                <version id="2.30">
                    <bitrate sixtyfourbit="true">
                        <filelocation>https://github.com/operasoftware/operachromiumdriver/releases/download/v.2.30/operadriver_mac64.zip</filelocation>
                        <hash>2b5273762c7f7c3d8ad61c03f7cbf4d0825b918b</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                </version>
            </driver>
            <driver id="phantomjs">
                <version id="1.9.8">
                    <bitrate thirtytwobit="true" sixtyfourbit="true">
                        <filelocation>https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-1.9.8-macosx.zip</filelocation>
                        <hash>d70bbefd857f21104c5961b9dd081781cb4d999a</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                </version>
            </driver>
            <driver id="marionette">
                <version id="0.19.1">
                    <bitrate thirtytwobit="true" sixtyfourbit="true">
                        <filelocation>https://github.com/mozilla/geckodriver/releases/download/v0.19.1/geckodriver-v0.19.1-macos.tar.gz</filelocation>
                        <hash>ae350f4510bbc0a8526f3fbfced8065b1b8586b2</hash>
                        <hashtype>sha1</hashtype>
                    </bitrate>
                </version>
            </driver>
        </osx>
    </root>