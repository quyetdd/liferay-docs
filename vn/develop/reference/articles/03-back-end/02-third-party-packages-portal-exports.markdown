---
header-id: third-party-packages-portal-exports
---

# Third Party Packages Portal Exports

[TOC levels=1-4]

The `com.liferay.portal.bootstrap` module exports many third party Java packages
that can cause problems if used improperly. If your WAR's Gradle file, for
example, uses the `compile` scope for a dependency that Liferay's OSGi runtime
already provides, the dependency JAR is included in the WAR's `WEB-INF/lib` and
deployed in the resulting WAB, and two versions of dependency classes wind up on
the classpath. This can cause weird errors that are hard to debug.

To find a list of the packages exported by `com.liferay.portal.bootstrap`, go to
the source file `modules/core/portal-bootstrap/system.packages.extra.bnd`. If
you don't have access to the source code, the same list (in a less user-friendly
format) is in the `META-INF/system.packages.extra.mf` file in
`[LIFERAY_HOME]/osgi/core/com.liferay.portal.bootstrap.jar`. These packages are
installed and available in Liferay's OSGi runtime. If your module or WAR uses
one of them, specify the corresponding dependency as being "provided" (provided
by @product@). Here's how to specify a provided dependency:

Maven: `<scope>provided</scope>`
 
Gradle: `providedCompile`

Now you can safely leverage third party packages @product@ provides! 

## Related topics

[Resolving a Plugin's Dependencies](/docs/7-1/tutorials/-/knowledge_base/t/resolving-a-plugins-dependencies)

[Configuring Dependencies](/docs/7-1/tutorials/-/knowledge_base/t/configuring-dependencies)
