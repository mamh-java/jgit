load(
    "@com_googlesource_gerrit_bazlets//tools:genrule2.bzl",
    "genrule2",
)
load("@rules_java//java:defs.bzl", "java_import", "java_library")
load(":tests.bzl", "tests")

PKG = "tst/org/eclipse/jgit/"

HELPERS = glob(
    ["src/**/*.java"],
    exclude = ["src/org/eclipse/jgit/transport/ssh/*.java"],
) + [PKG + c for c in [
    "api/AbstractRemoteCommandTest.java",
    "diff/AbstractDiffTestCase.java",
    "diff/AbstractRenameDetectionTestCase.java",
    "internal/diffmergetool/ExternalToolTestCase.java",
    "internal/revwalk/ObjectReachabilityTestCase.java",
    "internal/revwalk/ReachabilityCheckerTestCase.java",
    "internal/storage/file/GcTestCase.java",
    "internal/storage/file/PackIndexTestCase.java",
    "internal/storage/file/XInputStream.java",
    "nls/GermanTranslatedBundle.java",
    "nls/MissingPropertyBundle.java",
    "nls/NoPropertiesBundle.java",
    "nls/NonTranslatedBundle.java",
    "revwalk/RevQueueTestCase.java",
    "revwalk/RevWalkTestCase.java",
    "transport/ObjectIdMatcher.java",
    "transport/RequestValidatorTestCase.java",
    "transport/SpiTransport.java",
    "transport/InMemoryPack.java",
    "treewalk/filter/AlwaysCloneTreeFilter.java",
    "test/resources/SampleDataRepositoryTestCase.java",
    "util/CPUTimeStopWatch.java",
    "util/http/HttpCookiesMatcher.java",
    "util/io/Strings.java",
]]

DATA = [
    PKG + "lib/empty.gitindex.dat",
    PKG + "lib/sorttest.gitindex.dat",
]

EXCLUDED = [
    PKG + "api/SecurityManagerTest.java",
    PKG + "api/SecurityManagerMissingPermissionsTest.java",
    PKG + "lib/CommitTemplateConfigTest.java",
]

tests(tests = glob(
    ["tst/**/*.java"],
    exclude = HELPERS + DATA + EXCLUDED,
))


tests(tests = glob(["exttst/**/*.java"]),
    srcprefix = "exttst/",
    extra_tags = ["ext"])

# Non abstract base classes used for tests by other test classes
BASE = [
    PKG + "internal/storage/file/FileRepositoryBuilderTest.java",
    PKG + "internal/storage/file/RefDirectoryTest.java",
]

java_library(
    name = "base",
    testonly = 1,
    srcs = BASE,
    deps = [
        "//lib:junit",
        "//org.eclipse.jgit:jgit",
        "//org.eclipse.jgit.junit:junit",
    ],
)

java_library(
    name = "helpers",
    testonly = 1,
    srcs = HELPERS,
    resources = DATA,
    deps = [
        "//lib:assertj-core",
        "//lib:commons-codec",
        "//lib:commons-io",
        "//lib:junit",
        "//lib:mockito",
        "//lib:slf4j-simple",
        "//org.eclipse.jgit:jgit",
        "//org.eclipse.jgit.junit:junit",
    ],
)

java_import(
    name = "tst_rsrc",
    jars = [":tst_rsrc_jar"],
)

genrule2(
    name = "tst_rsrc_jar",
    srcs = glob(["tst-rsrc/**"]),
    outs = ["tst_rsrc.jar"],
    cmd = "o=$$PWD/$@ && tar cf - $(SRCS) | tar -C $$TMP --strip-components=2 -xf - && cd  $$TMP && zip -qr $$o .",
)
