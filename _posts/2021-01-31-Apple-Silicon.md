# Apple Silicon

It has been about a month since I purchased a new Apple MacBook Air with an M1 processor. The machine was purchased for personal use, so I don't use it for a lot of development, but I have used it as a trial run for future professional purchases. Most of the reviews for it have been spot on. The CPU performance has been amazing, the battery life is unprecedented, and the thermals... are the biggest win. I have yet to feel MacBook Air get warm, as opposed to my 2017 MacBook Pro that seems to get hot just by opening the lid. 

I don't have any major issues with the MacBook Air. However, there are a few things that are preventing me from moving to Apple Silicon for my professional development in the near future. I hope that my experience could possibly help someone else looking to make the jump to Apple Silicon. 


## OpenJDK

I use applications (e.g., Kafka) and develop applications that depend on OpenJDK. So it is nice that Java runs well within [Rosetta](https://developer.apple.com/documentation/apple_silicon/about_the_rosetta_translation_environment). However, for applications that are performance critical, using a dynamic binary translator such as Rosetta isn't an option. Oracle does have an early access preview (EAP) of [OpenJDK 16](https://github.com/microsoft/openjdk-aarch64/releases/tag/16-ea%2B10-macos) that supports Apple Silicon, but it is well... an EAP, and some of my clients still require Java 11 and in some cases... hold your breath, require Java 8. Thankfully, [Azul](https://www.azul.com/downloads/zulu-community/?package=jdk) has released an implementation of OpenJDK that natively supports Apple Silicon and provides backward compatible versions of JDK 8 and JDK 11. If you need a JDK that supports Apple Silicon in the near term, I would go with Azul. Azul is the JDK used on Azure, so it should be production ready.

## Testcontainers


Using containers for software development is almost a must nowadays, and the introduction of [Testcontainers](https://www.testcontainers.org) have been a blessing for writing integration tests. Testcontainers works with Rosetta, but the performance isn't great. So, if you do decide to run a version of the JDK that natively supports Apple Silicon, you will be left with a surprising error message that looks like the following below.


```console
[INFO] Running dev.seanking.testcontainer.IsaServiceIT
2021-01-31 11:07:33.012 ERROR 3945 --- [           main] o.t.d.DockerClientProviderStrategy       : Could not find a valid Docker environment. Please check configuration. Attempted configurations were:
2021-01-31 11:07:33.013 ERROR 3945 --- [           main] o.t.d.DockerClientProviderStrategy       :     UnixSocketClientProviderStrategy: failed with exception RuntimeException (java.lang.UnsatisfiedLinkError: /Users/username/Library/Caches/JNA/temp/jna6800472904820866591.tmp: dlopen(/Users/username/Library/Caches/JNA/temp/jna6800472904820866591.tmp, 1): no suitable image found.  Did find:
	/Users/username/Library/Caches/JNA/temp/jna6800472904820866591.tmp: no matching architecture in universal wrapper
	/Users/username/Library/Caches/JNA/temp/jna6800472904820866591.tmp: no matching architecture in universal wrapper). Root cause UnsatisfiedLinkError (/Users/username/Library/Caches/JNA/temp/jna6800472904820866591.tmp: dlopen(/Users/username/Library/Caches/JNA/temp/jna6800472904820866591.tmp, 1): no suitable image found.  Did find:
	/Users/username/Library/Caches/JNA/temp/jna6800472904820866591.tmp: no matching architecture in universal wrapper
	/Users/username/Library/Caches/JNA/temp/jna6800472904820866591.tmp: no matching architecture in universal wrapper)
2021-01-31 11:07:33.014 ERROR 3945 --- [           main] o.t.d.DockerClientProviderStrategy       :     UnixSocketClientProviderStrategy: failed with exception RuntimeException (java.lang.NoClassDefFoundError: Could not initialize class org.testcontainers.shaded.com.github.dockerjava.okhttp.UnixSocketFactory$1). Root cause NoClassDefFoundError (Could not initialize class org.testcontainers.shaded.com.github.dockerjava.okhttp.UnixSocketFactory$1)
2021-01-31 11:07:33.014 ERROR 3945 --- [           main] o.t.d.DockerClientProviderStrategy       : As no valid configuration was found, execution cannot continue
2021-01-31 11:07:33.016 ERROR 3945 --- [           main] o.s.boot.SpringApplication               : Application run failed
```

With some digging I was able to discover that Testcontainers depends on the [Java Native Access](https://github.com/java-native-access/jna) (JNA) library and JNA doesn't currently support Apple Silicon. The good news is that they are working on the feature, the bad news is there doesn't seem to be a release date as of now. To stay up-to-date on the issue, you can follow this open [ticket](https://github.com/java-native-access/jna/pull/1238) on the JNA repository on GitHub.

## Homebrew

Most mac developers I know use [Homebrew](https://brew.sh) to install apps and scripts (i.e., httpie, jenv, openjdk, wget, jenv, and more) on the Mac. I counted 94 packages are installed on my system, and I would probably guess that I am on the lower end compared to other developers. 

The good news is that Homebrew is adding support for Apple Silicon. The bad news is the support is in the early stages of supporting Apple Silicon. Currently, there are a few hurdles that must be overcome to use Homebrew on Apple Silicon. To prepare you, Homebrew currently recommends installing the Apple Silicon native version of Homebrew in `/opt/hombrew` and forbids installing it in `/usr/local`, to avoid clashing with the Intel version. There's also documentation for configuring two Terminals, one for apps natively and another for running apps in Rosetta. This bar is too high for me, so I will wait for it to mature some more before I proceed with Homebrew for Apple Silicon.   

## Virtualization / Emulation

Let's face it, x86 software and hardware aren't going away anytime soon, and many developers still need to develop for them. This means that anyone who wants to transition to Apple Silicon, will need software to emulate the x86 platform, or a second x86 computer. As of writing this, neither [Parallels](https://www.parallels.com) or [VMWare Fusion](https://www.vmware.com/products/fusion.html) have committed to releasing a version of their software that can run on Apple Silicon and emulate x86 hardware. I really hope one of the companies commit to adding support for x86 hardware emulation soon, because I would prefer not having to maintain another machine just for legacy hardware :wink: :laughing:. 

# Conclusion

Excluding the virtualization of x86 hardware, I imagine that my other issues will be resolved soon and once they are I will transition to Apple Silicon for professional development. I am really excited for Apple Silicon, and for other ARM processors in the future. It appears that desktop class ARM processors will push a once stagnant industry forward again.  
