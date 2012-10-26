cmake-multi-package
===================

Sample cmake project that generates multiple packages.

To build::

    mkdir release; cd release; cmake ..; make; make pkg

This example was inspired by
https://github.com/shadowmint/cmake-multi-install.  But the
cmake-multi-install example has a couple of shortcomings:

* It has multiple levels of indirection where only one level is
  required.

* It uses add_custom_target() to create a "deb" target, but the
  DEPENDS option of add_custom_target() is meant for file-level
  dependencies only; add_dependencies() should be used for
  target-level ones.  In other words, we must use this::

    add_custom_target(deb)
    add_dependencies(deb package-demo1 package-demo2)

  instead of::

    add_custom_target(deb DEPENDS package-demo1 package-demo2)

  Improper use of add_custom_target() can result in inexplicable "No
  rule to make target" errors.

In addition, this example has some additional features:

* It shows how to use multiple generators like DEB, RPM, and
  PackageMaker.  If you run "make pkg", the correct generator is
  selected for your platform.

* It shows how to deal with SOVERSION issues when installing shared
  libraries.
